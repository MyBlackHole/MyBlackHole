# tauri


```shell
cargo install create-tauri-app --locked
cargo create-tauri-app

cd tauri-app
npm install
npm run tauri dev

<!--npm run tauri android dev-->
```

- 使用 Tauri CLI 手动创建
```shell
mkdir tauri-app
cd tauri-app
npm create vite@latest .

npm install -D @tauri-apps/cli@latest
<!--http://localhost:5173-->

npx tauri init

✔ What is your app name? tauri-app
✔ What should the window title be? tauri-app
✔ Where are your web assets located? ..
✔ What is the url of your dev server? http://localhost:5173
✔ What is your frontend dev command? pnpm run dev
✔ What is your frontend build command? pnpm run build


npx tauri dev
```
