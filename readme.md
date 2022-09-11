40行代码完成v2ph爬取数据


练手爬虫的项目，使用网页自动化的方式，很稳定， 适合数据量不大的情况，不用太担心反爬措施。
而且使用的是用户浏览器， 更加真实，不易被反扒。
开发环境参考[visual studio code extension](https://marketplace.visualstudio.com/items?itemName=ClickCorp.clicknium)

- 使用[clicknium](https://www.clicknium.com/documents/tutorial/recorder/capture_similar_elements) 获取相似元素功能，可以直接获取到所有的照片列表

- 然后访问单个照片集页面
- 继续通过获取相似元素功能，获取到当页照片的列表
- 遍历获取url，然后通过requests下载照片
  
```python
headers = {
            'referer': 'https://www.v2ph.com/',
            'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36'
        }
        for img in imgs:
            url = img.get_property("src")
            file_name = url.split("/")[-1]
            img_file = requests.get(url, headers=headers)
            temp_file = os.path.join(os.getcwd(), "picture", "{}".format(file_name))
            open(temp_file, 'wb').write(img_file.content)
            sleep(random.randint(1,5))
```
这里需要设置好header，尤其是referer
- 最后通过requests进行下载
- 单页照片都下载完成后，判断是否有下一页，有的话，自动翻页，然后同样方式抓取下一页照片


注意：
人机交互的验证以及登录没有去做自动化，用户可以自己先注册和手动访问一下。
