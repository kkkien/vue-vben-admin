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

chỗ nào từ UI call tới api port 5173, dùng port khác thay 5173 cho server data thế nào?


Flow
  - src\router\routes\index.ts
    import menu "./modules/**/*.ts", các file .ts trong đường dẫn này publish AppRouteModule là các menu 