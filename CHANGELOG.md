# [1.7.0](https://github.com/databk/rustdesk-console-web/compare/1.6.1...1.7.0) (2026-10-18)


### Bug Fixes

* align frontend API types with backend definitions ([#348](https://github.com/databk/rustdesk-console-web/issues/348)) ([23acde8](https://github.com/databk/rustdesk-console-web/commit/23acde89810a2b7639a9e46b67731252652c9a9e))
* **ci:** use App token for release push ([#354](https://github.com/databk/rustdesk-console-web/issues/354)) ([42ba610](https://github.com/databk/rustdesk-console-web/commit/42ba6101d75d0714648f5a869a4a83bdca0252ae))
* **dashboard:** use dashboard.view permission, add device name search and CSV export ([#357](https://github.com/databk/rustdesk-console-web/issues/357)) ([3752ae2](https://github.com/databk/rustdesk-console-web/commit/3752ae29c8ddedaef1aa667e17f2ba33cf22bdac)), closes [#415](https://github.com/databk/rustdesk-console-web/issues/415)
* **login:** render alternative login methods below the Login button ([#369](https://github.com/databk/rustdesk-console-web/issues/369)) ([60e0b29](https://github.com/databk/rustdesk-console-web/commit/60e0b29e0e61e18ea9a8cc4f62025fe9f51710f2))


### Features

* add hbbs and hbbr management dashboard ([#353](https://github.com/databk/rustdesk-console-web/issues/353)) ([1191035](https://github.com/databk/rustdesk-console-web/commit/11910356a94b4c40683ebaa2b73a07048465a32f))
* add one-click coordinated system updates to the WebUI ([#352](https://github.com/databk/rustdesk-console-web/issues/352)) ([290dc24](https://github.com/databk/rustdesk-console-web/commit/290dc240bddc079e81091976f83c7a64b0933642))
* **login:** add forgot-password flow with reset page ([#371](https://github.com/databk/rustdesk-console-web/issues/371)) ([36db500](https://github.com/databk/rustdesk-console-web/commit/36db5008602795e7e4e6e7405c5ee0e239055037))
* **settings:** add web client and server management settings pages ([#373](https://github.com/databk/rustdesk-console-web/issues/373)) ([6965ed2](https://github.com/databk/rustdesk-console-web/commit/6965ed2dd43db67ee3fbd057de23ae3fc649d50c))
* **web-client:** add an experimental browser remote control client ([#351](https://github.com/databk/rustdesk-console-web/issues/351)) ([c9f0e89](https://github.com/databk/rustdesk-console-web/commit/c9f0e89028c64d9b68015c61a30b60ebbde7082d))


### Reverts

* Revert "chore(deps-dev): bump @umijs/max from 4.7.22 to 4.7.23 (#363)" (#364) ([3c2c41f](https://github.com/databk/rustdesk-console-web/commit/3c2c41f86cc66f229fbf01f439c7f68bf78c6e5a)), closes [#363](https://github.com/databk/rustdesk-console-web/issues/363) [#364](https://github.com/databk/rustdesk-console-web/issues/364)



## [1.6.1](https://github.com/databk/rustdesk-console-web/compare/1.6.0...1.6.1) (2026-10-01)


### Bug Fixes

* respect backend pageSize limit of 100 in remaining callers ([#345](https://github.com/databk/rustdesk-console-web/issues/345)) ([c6305c9](https://github.com/databk/rustdesk-console-web/commit/c6305c94bf4f980b76c08e3ab8dec27c7f38ab8e))
* **users:** prevent system owner status and deletion actions ([#341](https://github.com/databk/rustdesk-console-web/issues/341)) ([a77a85f](https://github.com/databk/rustdesk-console-web/commit/a77a85f29ca46d498142c978199b1a2f80e2f711))
* **users:** remove obsolete admin toggle from edit user modal ([#340](https://github.com/databk/rustdesk-console-web/issues/340)) ([2254ac4](https://github.com/databk/rustdesk-console-web/commit/2254ac4efeb44a029d24826f27b7bb7d8d0f367e)), closes [#379](https://github.com/databk/rustdesk-console-web/issues/379)



# [1.6.0](https://github.com/databk/rustdesk-console-web/compare/1.5.1...1.6.0) (2026-09-27)


### Bug Fixes

* configure public path for GitHub Pages ([#323](https://github.com/databk/rustdesk-console-web/issues/323)) ([6d634ba](https://github.com/databk/rustdesk-console-web/commit/6d634baeffa25792737a797c658fe6f99e3395c4))
* **dashboard:** align trend card height with left cards ([#328](https://github.com/databk/rustdesk-console-web/issues/328)) ([cce6a7d](https://github.com/databk/rustdesk-console-web/commit/cce6a7d68fc74c0e24f9b88813c96e047a2a36a8))
* **dashboard:** fetch trend data on initial mount ([#327](https://github.com/databk/rustdesk-console-web/issues/327)) ([9c7d695](https://github.com/databk/rustdesk-console-web/commit/9c7d695b05bf32e782f2666c77229d11336c1e27))
* **dashboard:** improve color hierarchy and card readability ([#331](https://github.com/databk/rustdesk-console-web/issues/331)) ([146e40c](https://github.com/databk/rustdesk-console-web/commit/146e40c88f411fae8d5dd4b7a7be893ce6ac915b))
* **devices:** use case-sensitive connect URL and add connection method menu ([#320](https://github.com/databk/rustdesk-console-web/issues/320)) ([8c2861f](https://github.com/databk/rustdesk-console-web/commit/8c2861f51c3fd5ffbab72dd035e107e008f16318))
* **docker:** add --legacy-peer-deps flag to npm ci in Dockerfile ([#319](https://github.com/databk/rustdesk-console-web/issues/319)) ([f5569a6](https://github.com/databk/rustdesk-console-web/commit/f5569a60ce50abcf53c41b399eb7ea58dc560951)), closes [#317](https://github.com/databk/rustdesk-console-web/issues/317)
* **i18n:** replace hardcoded text in console audit page ([#334](https://github.com/databk/rustdesk-console-web/issues/334)) ([9ac019f](https://github.com/databk/rustdesk-console-web/commit/9ac019f733165b20db0c40d618d6c83c61c7af99))
* **locale:** include fr-FR in tests and ru-RU in language options ([#298](https://github.com/databk/rustdesk-console-web/issues/298)) ([d5c6cd6](https://github.com/databk/rustdesk-console-web/commit/d5c6cd6dfea7c18c514bb5cc9e7d20a4330037eb))
* **login:** use uniform 24px spacing between fields and the Passkey/Login buttons ([#335](https://github.com/databk/rustdesk-console-web/issues/335)) ([40dc789](https://github.com/databk/rustdesk-console-web/commit/40dc78964661f7df2f21bb53d25df4121d238c73))
* **oidc:** sanitize SVG icons with DOMPurify instead of data URI ([#312](https://github.com/databk/rustdesk-console-web/issues/312)) ([2956c44](https://github.com/databk/rustdesk-console-web/commit/2956c443938d2b8fde8e6fa0bc4988e7ac9124b9))
* remove incorrect CNAME and update logo to rustdesk official ([#330](https://github.com/databk/rustdesk-console-web/issues/330)) ([6f8ed58](https://github.com/databk/rustdesk-console-web/commit/6f8ed58ff3297066da7b7c51fd96701cb00f3f0c))
* remove SVG width/height attributes and strip empty URL fields ([#315](https://github.com/databk/rustdesk-console-web/issues/315)) ([046f2b2](https://github.com/databk/rustdesk-console-web/commit/046f2b2c6d0ba76166fbcbbd69aecead4c32990d))
* **roles:** remove index column from roles list table ([#303](https://github.com/databk/rustdesk-console-web/issues/303)) ([adbdcfb](https://github.com/databk/rustdesk-console-web/commit/adbdcfb2ca938f07126926915bde7cdff85ded31))
* **strategy:** remove builtin-only options from strategy page ([#301](https://github.com/databk/rustdesk-console-web/issues/301)) ([47fdfd9](https://github.com/databk/rustdesk-console-web/commit/47fdfd91d311d0b14f3733b6cb7713180b72ec72))
* support GitHub Pages routing ([#324](https://github.com/databk/rustdesk-console-web/issues/324)) ([983f628](https://github.com/databk/rustdesk-console-web/commit/983f6280b10ba4a6a594569d6cc00b48ed4273af))
* use cargo-cross for aarch64-musl build instead of musl.cc ([#310](https://github.com/databk/rustdesk-console-web/issues/310)) ([2582ef0](https://github.com/databk/rustdesk-console-web/commit/2582ef02b935b8ea2a665a0fb04befb2fa7fe40a))


### Features

* add musl binary build targets for x86_64 and aarch64 ([#309](https://github.com/databk/rustdesk-console-web/issues/309)) ([a9acb02](https://github.com/databk/rustdesk-console-web/commit/a9acb020fcc9f17949ebe29fc9d5bb4097c7eeb0))
* add Russian (ru-RU) localization ([#276](https://github.com/databk/rustdesk-console-web/issues/276)) ([b998b2e](https://github.com/databk/rustdesk-console-web/commit/b998b2e07a21fb7ed6c56c3bb8f379ca2de59340))
* **audit:** adapt frontend to new audit fields from backend ([#318](https://github.com/databk/rustdesk-console-web/issues/318)) ([7452a3c](https://github.com/databk/rustdesk-console-web/commit/7452a3c57ba669725f7fef6b61502d78513601c7))
* **audit:** expose console audit log ([#322](https://github.com/databk/rustdesk-console-web/issues/322)) ([44af1f7](https://github.com/databk/rustdesk-console-web/commit/44af1f76887b02e85167880b5e85aef4e352bd35))
* **dashboard:** redesign dashboard UI with ring charts, combined trend chart, and unified API ([#325](https://github.com/databk/rustdesk-console-web/issues/325)) ([4beb5fb](https://github.com/databk/rustdesk-console-web/commit/4beb5fbf16e1bc5a8e36ad00d4b0409a42626231)), closes [#13c2c2](https://github.com/databk/rustdesk-console-web/issues/13c2c2) [#361](https://github.com/databk/rustdesk-console-web/issues/361)
* **locale:** add fr-FR (French) language support ([#297](https://github.com/databk/rustdesk-console-web/issues/297)) ([e97776c](https://github.com/databk/rustdesk-console-web/commit/e97776cbf876e90d5fc848147444089ba29c7d11)), closes [#296](https://github.com/databk/rustdesk-console-web/issues/296)
* **oidc:** adapt monochrome icons to theme and strip preset field from API calls ([#313](https://github.com/databk/rustdesk-console-web/issues/313)) ([d82fe8a](https://github.com/databk/rustdesk-console-web/commit/d82fe8a24c097c6e6076b0dfe275bdcad30fa592))
* **oidc:** hide built-in config fields when selecting a preset provider ([#314](https://github.com/databk/rustdesk-console-web/issues/314)) ([fe530c8](https://github.com/databk/rustdesk-console-web/commit/fe530c864446e0b0f82d569c5bdd4c4044987f45))
* **oidc:** unify provider icons with client and support SVG upload ([#308](https://github.com/databk/rustdesk-console-web/issues/308)) ([9970d11](https://github.com/databk/rustdesk-console-web/commit/9970d11cf47acdada5a5d115cc372eee75214331))
* **rbac:** add permission-aware administration UI ([#281](https://github.com/databk/rustdesk-console-web/issues/281)) ([8b3a977](https://github.com/databk/rustdesk-console-web/commit/8b3a97703dca90908e7675298caed021619f56b3))
* **settings:** add jwtExpiryDays and auditRetentionDays to general settings ([#300](https://github.com/databk/rustdesk-console-web/issues/300)) ([8ad97e7](https://github.com/databk/rustdesk-console-web/commit/8ad97e7259a57306946a3d9d3cc89249e401804e))
* **settings:** use two-column masonry layout for general settings ([#321](https://github.com/databk/rustdesk-console-web/issues/321)) ([d9fd1ff](https://github.com/databk/rustdesk-console-web/commit/d9fd1ff26b2389cc1dfab013a2441d6d3f86393e))
* **strategy:** localize strategy configuration options ([#293](https://github.com/databk/rustdesk-console-web/issues/293)) ([265ece7](https://github.com/databk/rustdesk-console-web/commit/265ece73c03a34d079a9bbfc4228d2437606d632))
* **users:** add admin password reset in security modal ([#299](https://github.com/databk/rustdesk-console-web/issues/299)) ([d4c9c1e](https://github.com/databk/rustdesk-console-web/commit/d4c9c1e533b48dbb83cfbf17ef704ce5c1099774))



## [1.5.1](https://github.com/databk/rustdesk-console-web/compare/1.5.0...1.5.1) (2026-08-12)


### Bug Fixes

* **i18n:** add missing pt-BR translations and remove unused keys ([#270](https://github.com/databk/rustdesk-console-web/issues/270)) ([7c6249d](https://github.com/databk/rustdesk-console-web/commit/7c6249d1ea544fd92e5d8bcc818026e69215af3a))
* localize /user/center title via explicit route locale ([#265](https://github.com/databk/rustdesk-console-web/issues/265)) ([43b06ab](https://github.com/databk/rustdesk-console-web/commit/43b06ab9d8180f3ccef3dcf3ad324f9855716a09))
* prevent page title suffix from leaking into PageContainer ([#263](https://github.com/databk/rustdesk-console-web/issues/263)) ([d39b7f7](https://github.com/databk/rustdesk-console-web/commit/d39b7f78c2f9b020f5aa5388615411b09385daf6)), closes [#261](https://github.com/databk/rustdesk-console-web/issues/261)



# [1.5.0](https://github.com/databk/rustdesk-console-web/compare/1.4.0...1.5.0) (2026-08-07)


### Bug Fixes

* add target suffix to binary names to prevent overwrite on upload ([#233](https://github.com/databk/rustdesk-console-web/issues/233)) ([3503339](https://github.com/databk/rustdesk-console-web/commit/350333913d6c18a7e160b7722286371c92e09e83))
* **address-book:** remove unsupported recycle bin feature ([#241](https://github.com/databk/rustdesk-console-web/issues/241)) ([131fd32](https://github.com/databk/rustdesk-console-web/commit/131fd32c0cbd61a5db77c7e60a23ee6048191dc5))
* **build:** disable sourcemap in production build when using mako ([#252](https://github.com/databk/rustdesk-console-web/issues/252)) ([1d11da0](https://github.com/databk/rustdesk-console-web/commit/1d11da01ae277d65f3aebdeb48d6a34f8abc3e8a))
* change update check API from POST to GET with query params ([#237](https://github.com/databk/rustdesk-console-web/issues/237)) ([ae56de6](https://github.com/databk/rustdesk-console-web/commit/ae56de6d00128798bf463c62c37392c26442e9b1))
* **devices:** display version, cpu and memory in Info column instead of redundant OS info ([#242](https://github.com/databk/rustdesk-console-web/issues/242)) ([f3aa35f](https://github.com/databk/rustdesk-console-web/commit/f3aa35ff8205728dc2ad85e141c0ab4f70d76e3d))
* **devices:** reduce status column width and narrow ID column ([#243](https://github.com/databk/rustdesk-console-web/issues/243)) ([831f7f5](https://github.com/databk/rustdesk-console-web/commit/831f7f5db8aa0fb4a67990a85b29d6152ecacf77))
* dynamically adjust device action column width based on button count ([#248](https://github.com/databk/rustdesk-console-web/issues/248)) ([cd9b2c6](https://github.com/databk/rustdesk-console-web/commit/cd9b2c6a8474285f9048847cb774bd88ca4ff2e1))
* hardcode Docker Hub username as databk in workflow files ([#247](https://github.com/databk/rustdesk-console-web/issues/247)) ([5b8f90e](https://github.com/databk/rustdesk-console-web/commit/5b8f90e1c4e9562a605c1cc3e8161ad1a0da4bd3))
* prevent spurious 401 'Login expired' prompt on logout ([#253](https://github.com/databk/rustdesk-console-web/issues/253)) ([1edab6c](https://github.com/databk/rustdesk-console-web/commit/1edab6c13c5fc9042dff393e95cf8f0742ae9fc3))
* reduce connection audit table width to prevent horizontal scrollbar ([#246](https://github.com/databk/rustdesk-console-web/issues/246)) ([9efc2f9](https://github.com/databk/rustdesk-console-web/commit/9efc2f95decd896bd9140b73ee1c11c11e7d9084))
* **routes:** hoist /user/login to top-level route to restore layout:false ([#260](https://github.com/databk/rustdesk-console-web/issues/260)) ([e3cb940](https://github.com/databk/rustdesk-console-web/commit/e3cb940e47629b0f69b54db03bede8bca2ab1850))
* skip update-check on login page to avoid false 401 alert ([#213](https://github.com/databk/rustdesk-console-web/issues/213)) ([1509f77](https://github.com/databk/rustdesk-console-web/commit/1509f7712741adcac491d45cbdfce50164bd51a3))
* use macos-26-intel runner for x86_64-apple-darwin build ([#259](https://github.com/databk/rustdesk-console-web/issues/259)) ([876f199](https://github.com/databk/rustdesk-console-web/commit/876f1993d179eb103539e693f9c538e5567769b9))
* **user-groups:** remove unsupported note search from user group list ([#239](https://github.com/databk/rustdesk-console-web/issues/239)) ([f5f5e6c](https://github.com/databk/rustdesk-console-web/commit/f5f5e6ca366d4d20af6990aeef9f8af20c7fc4b9))
* **users:** remove horizontal scrollbar by making table columns responsive ([#236](https://github.com/databk/rustdesk-console-web/issues/236)) ([c6bbfab](https://github.com/databk/rustdesk-console-web/commit/c6bbfab1e0178e291803844c818f6154ebc4bf5a))


### Features

* add login session management ([#205](https://github.com/databk/rustdesk-console-web/issues/205)) ([4bc686f](https://github.com/databk/rustdesk-console-web/commit/4bc686ffec09d8d3de2b939c1cae0b4aa94b069c))
* add Passkey (WebAuthn) frontend integration ([#204](https://github.com/databk/rustdesk-console-web/issues/204)) ([0b4fa01](https://github.com/databk/rustdesk-console-web/commit/0b4fa015f69c6abeda099f88bc1f4e70c69049f9))
* add PWA support for installable web app ([#217](https://github.com/databk/rustdesk-console-web/issues/217)) ([c332aa7](https://github.com/databk/rustdesk-console-web/commit/c332aa711fa0bf3a77c883cf49e68939f88f5d7c))
* add standalone executable support with Rust embedded web server ([#198](https://github.com/databk/rustdesk-console-web/issues/198)) ([8773295](https://github.com/databk/rustdesk-console-web/commit/877329542cb88dd00e87626fd7b6789f36686e2e))
* **ci:** add nightly build workflow ([#215](https://github.com/databk/rustdesk-console-web/issues/215)) ([2c4233f](https://github.com/databk/rustdesk-console-web/commit/2c4233fcfe8ba8ac58c97a4de02e9a4db1445613))
* **config:** enable code splitting with granularChunks strategy ([#251](https://github.com/databk/rustdesk-console-web/issues/251)) ([4adbf1a](https://github.com/databk/rustdesk-console-web/commit/4adbf1aa7526ac1dead3bcd156ce98b6e6be7a2d))
* **custom-client:** add retry button for failed builds ([#235](https://github.com/databk/rustdesk-console-web/issues/235)) ([62e40ed](https://github.com/databk/rustdesk-console-web/commit/62e40ed72b05a6c75928cfba54dade2e45ed0fb9))
* **device-groups:** add name search functionality to device group list ([#240](https://github.com/databk/rustdesk-console-web/issues/240)) ([321cff0](https://github.com/databk/rustdesk-console-web/commit/321cff0d99c808ac591ed63df891607da32e8bf6))
* **devices:** make ID column clickable with rustdesk:// protocol link ([#244](https://github.com/databk/rustdesk-console-web/issues/244)) ([32d4b3b](https://github.com/databk/rustdesk-console-web/commit/32d4b3bf831289d9ca5371fa524baa14ceec6d62))
* **groups:** make user group name clickable to open group user list ([#218](https://github.com/databk/rustdesk-console-web/issues/218)) ([1a97fe3](https://github.com/databk/rustdesk-console-web/commit/1a97fe303073393aa90f18ce2606cf1bb5ff76e5))
* **server:** update default listen port to 21114 and backend URL port to 3000 ([#229](https://github.com/databk/rustdesk-console-web/issues/229)) ([301c14d](https://github.com/databk/rustdesk-console-web/commit/301c14dfbfe18d2103fbd2bfa7aec1c97a68cd1f))
* set random default color for new tags ([#245](https://github.com/databk/rustdesk-console-web/issues/245)) ([838c9e0](https://github.com/databk/rustdesk-console-web/commit/838c9e0afe2f40778062462293fa2cd3a0b8f2ce))
* **settings:** support expanded general settings contract ([#249](https://github.com/databk/rustdesk-console-web/issues/249)) ([53bdc5a](https://github.com/databk/rustdesk-console-web/commit/53bdc5aa7e323bed35218606c6f55e8380c67414))
* **settings:** use public /settings/frontend for bootstrap config ([#250](https://github.com/databk/rustdesk-console-web/issues/250)) ([b3dd7b2](https://github.com/databk/rustdesk-console-web/commit/b3dd7b20adb6c81f6062c0b4dd9224984c859670))
* **users:** add user_group_name, strategy_name and is_admin query params ([#238](https://github.com/databk/rustdesk-console-web/issues/238)) ([3f6380c](https://github.com/databk/rustdesk-console-web/commit/3f6380c619a600c25b3fdd73f68b70af122e88ed))



