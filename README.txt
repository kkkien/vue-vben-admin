11/05/2023

mock component public API cùng port 5173
  - test browser/postman: http://localhost:5173/basic-api/account/getAccountInfo
disable mock:
  - internal\vite-config\src\plugins\index.ts
=========================================================
menu: Component - Card List
url: http://localhost:5173/#/comp/cardList

UI: src\views\demo\comp\card-list\index.vue
-> file index import src\api\demo\table.ts
cách call mock -> api: /basic-api/table/getDemoList
  - mock\demo\table-demo.ts => cung cấp mock data cho => src\api\demo\table.ts


start
  - src\components\CardList\index.ts
    - src\components\CardList\src\CardList.vue

Delete
  - src\components\CardList\src\CardList.vue
  (có dropdown menu Delete)

=========================================================
menu: Component - Table - Tree
url: http://localhost:5173/#/comp/table/treeTable
=> ko get data từ mock, dùng data từ src\views\demo\table\tableData.tsx

=========================================================
chỗ nào từ UI call tới api port 5173, dùng port khác thay 5173 cho server data thế nào?
  - .env.production hoặc .env.development -> khai báo: VITE_GLOB_API_URL=/basic-api
    - src\hooks\setting\index.ts -> khai báo: apiUrl: VITE_GLOB_API_URL
      - src\utils\http\axios\index.ts -> dùng apiUrl chính là '/basic-api'
        - vite.config.ts -> khai báo server-proxy, point '/basic-api' đến target: 'http://localhost:3000'
  cũng xem thêm: src\utils\http\axios\index.ts -> dòng 236 createAxios ->  baseURL: globSetting.apiUrl,
=========================================================

Flow
  - src\router\routes\index.ts
    import menu "./modules/**/*.ts", các file .ts trong đường dẫn này publish AppRouteModule là các menu 

Production:
- disable mock module
  internal\vite-config\src\plugins\index.ts => hardcode enableMock = false
  hoặc
  internal\vite-config\src\config\application.ts => enableMock: VITE_USE_MOCK === 'false'
- disable all demo menus
  src\router\routes\index.ts => rem dòng 26 là đc

  thêm 1 proxy target trong vite.config.ts, trỏ '/my-api' tới VD http://localhost:8888 (server chạy api lấy data)
  trong .env.production, khai báo VITE_GLOB_API_URL=/my-api
  viết code server public API http://localhost:8888/my-api/...