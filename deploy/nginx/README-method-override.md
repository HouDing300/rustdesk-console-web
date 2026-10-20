# HTTP 方法伪装 / 还原（PUT、DELETE 被内网安全设备拦截的绕行方案）

## 现象

RustDesk Console 页面上「角色授权」「删除角色」「保存设置」等操作报错，
DevTools 显示 `net::ERR_CONNECTION_RESET`，而 GET / POST（如新建角色、新建用户）正常。

## 根因

客户端与服务器（192.168.60.41:21114）之间的网络安全设备只放行 GET / POST：

| 方法 | 从客户端发起的结果 |
| --- | --- |
| GET | 正常 |
| POST | 正常 |
| PATCH | 正常（多数设备默认不拦） |
| PUT | 连接被 RST，请求从未到达 nginx |
| DELETE | 连接被 RST，请求从未到达 nginx |

服务器侧完全正常：从服务器本机访问自己的 21114 端口，PUT / DELETE 均正常返回；
主机上无安全软件、iptables 无 string 匹配规则。因此是链路上的中间设备行为。

## 方案

1. **前端**：`src/requestErrorConfig.ts` 的 requestInterceptors 中，
   把 PUT / PATCH / DELETE 改写为 POST，并加 `X-HTTP-Method-Override: <原方法>` 头。
   项目内 **54 处**写方法调用（角色、用户、设备、设备组、用户组、策略、地址簿、
   审计、LDAP、OIDC、Passkey、SMTP、设置、Web 客户端）全部走 `@umijs/max` 的
   `request`，因此一处拦截即可全覆盖；`actionRequest` / `webClientRequest` 两个封装
   内部同样调用 `request`，也被覆盖。
2. **nginx**：根据 `X-HTTP-Method-Override` 还原真实方法后转发给后端，
   见 `00-method-override.conf`。

**两者必须配套部署**：只改前端不改 nginx，写操作会以 POST 抵达后端返回 404。

## 应用方式

```bash
git apply http-method-override.patch
cp 00-method-override.conf /etc/nginx/conf.d/
# 在站点 conf 的 location /api/ { 之后加入：
#   set $rd_real_method $request_method;
#   if ($rd_override_method != "") { set $rd_real_method $rd_override_method; }
#   proxy_method $rd_real_method;
nginx -t && systemctl reload nginx
npm run build   # 前端需重新构建才生效
```

## 验证

```bash
curl -X POST http://<host>:21114/api/settings/general \
  -H "Authorization: Bearer <token>" -H "Content-Type: application/json" \
  -H "X-HTTP-Method-Override: PUT" -d '{}'
# 期望返回 PUT 接口的业务校验错误（400 watermarkEnabled ...），
# 而不是 404 / 连接被重置 —— 即说明还原成功。
```
