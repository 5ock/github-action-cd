# Github Actions CD

      name: CD
      on:
        push:
          branches: [ main ]
      jobs:
        build:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2
            
- name: workflow name
- on: workflow 觸發條件, 例子當 push 到 master branch 就會觸發 Github Actions 的 update gist workflow
- jobs: job 由許多 steps 組成， 
  - build 是給 job 的名稱
  - runs-on 指定要運行的環境，這個專案是使用 ubuntu，還有 windows 與 macOS 可以選擇。
  - 多個 jobs 預設會是平行執行的，如果 jobs 間存在依賴關係，那它們的執行順序開發者也是可以自行指定的，如:

        jobs:
          job1:
          job2
            needs: job1

- steps: 定義 workflow 執行的指令。
  - uses: actions/checkout@v2 > 使用 <a href='https://github.com/actions/checkout'>checkout@v2 </a>


## 部署 FTP Demo
- Used <a href='https://github.com/kevinpainchaud/simple-ftp-deploy-action'>kevinpainchaud/simple-ftp-deploy</a>


      name: CD
      on:
        push:
          branches: [ main ]
      jobs:
        build:
          runs-on: ubuntu-latest
          steps:
            - uses: actions/checkout@v2
            - uses: actions/setup-node@v1
              with:
                node-version: 14
            - name: npm install
              run: npm install
            - name: build & export
              run: npm run export --if-present

            - name: ftp depoly
              uses: kevinpainchaud/simple-ftp-deploy-action@v1.2.1
              with:
                ftp_host: ${{ secrets.NCPM_FTP_HOST }}
                ftp_username: ${{ secrets.NCPM_FTP_USERNAME }}
                ftp_password: ${{ secrets.NCPM_FTP_PASSWORD }}
                local_source_dir: "out"
                dist_target_dir: "/"
                delete: "true"
                exclude: ""
