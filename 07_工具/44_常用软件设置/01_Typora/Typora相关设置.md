# 1. 快捷键设置

在Typora中，点击==文件==，在出现的下拉列表中选择==偏好设置==，此后按图操作

![image-20210517154648471](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/03_%E5%B7%A5%E5%85%B7/24_Typora/20210517163536.png)

使用文本编辑器打开文件 ==conf.user.json== 文件

```json
  "keyBinding": {
    // for example: 
    // "Always on Top": "Ctrl+Shift+P"
    // All other options are the menu items 'text label' displayed from each typora menu
    "Highlight":"Ctrl+Shift+H",	 // 高亮快捷键
    "Code":"Ctrl+`",   // 代码快捷键
  },
```

# 2. 主题设置

在Typora中，点击==文件==，在出现的下拉列表中选择==偏好设置==，此后按图操作

![image-20210517155101053](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/03_%E5%B7%A5%E5%85%B7/24_Typora/20210517163539.png)

这个文件夹就是主题，可以自己修改，也可以去官网下载

# 3. 设置引用多样式

## 3.1. 引用多种样式

我想将==引用==设置为多种样式，但是Typora仅仅支持一种，可以采用VLOOK插件实现，但是VLOOK仅支持导出之后生成。所以我对Typora进行了修改，最终实现的效果如下图所示。

![image-20210517155508702](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/03_%E5%B7%A5%E5%85%B7/24_Typora/20210517163544.png)

## 3.2. 缺陷

此种方式在==Typora version 0.10.10(beta)==下修改，其他版本是否有效未知，修改之前先备份

此种方式有两个缺陷，如果比较见意，那么就不用作

1. 自定义样式前面必须显式声明，有==\[警告\]\[推荐\]\[危险\]==，这个内容可以按自己要求来设置，在[3.3. 修改操作](#3.3. 修改操作)可以自己设置

   ![image-20210517162704901](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/03_%E5%B7%A5%E5%85%B7/24_Typora/20210517163541.png)

2. 在添加引用时，要注意

   - 默认/原来的引用，还是按之前的来 `> 这是需要引用的内容`
   - 自己定义的 ==\[警告\]\[推荐\]\[危险\]==的引用样式需要如下操作，两种操作任选一种
     - 先输入`[警告]`，再在它的前面添加`>`符号，就会出现想要的效果（*先输入`>`后，Typora就会渲染出默认样式，后面就不会渲染了，除非重启Typora*）
     - 直接输入`> [警告]`，但是颜色不会变（是默认引用），需要重启Typora之后才会渲染。建议第一种





## 3.3. 修改操作

步骤简单，主要修改两个文件，具体步骤如下：==修改前注意先备份文件==

1. 在主题css文件添加想要的样式。*这个样式自己想怎么改就怎么改*

   ```css
   .blockquote-tuijian {
       /* 推荐  蓝色*/
       border-left-color: #5bc0de;
       color: #5bc0de;
       background-color: #f4f8fa;
    }
   .blockquote-jinggao {
       /* 警告  黄色*/
       background-color: #fcf8f2;
       border-color: #f0ad4e;
       color: #f0ad4e;
    }
   .blockquote-weixian {
       /* 危险  红色*/
       color: #d9534f;
       background-color: #fdf7f7;
       border-color: #d9534f;
    }
   ```

   下图为上面设置的样式

   ![image-20210517155508702](https://java-picgo-img.oss-cn-beijing.aliyuncs.com/03_%E5%B7%A5%E5%85%B7/24_Typora/20210517163547.png)

2. Markdown在转换为HTML的时候，会将==引用==`>符号`转换为`<blockquote>标签`。由于没有`class、id选择器`，所以不能直接在==css==中设置自动渲染，这就需要在转换过程中进行设置，通过js动态绑定。

   Typora的MarkDown解析==js==文件在安装路径下，这是我的安装路径：`D:\Program Files\Typora\resources\appsrc\window\frame.js`。通过编辑器打开后，进行修改。

   一共有两处需要修改

   1. 修改实时渲染处：通过工具查询 `case o.blockquote:`，这个js文件中仅为此句

      - 原来为：

        ```javascript
        case o.blockquote:
                return "<blockquote " + f(this) + " >" + h(this) + "</blockquote>";
        ```

        

      - 修改为：如果不想设置为==\[警告\]\[推荐\]\[危险\]==，则需要在下面代码中自己修改

        ```javascript
        case o.blockquote:
            // 转换开始
            console.log(h(this).indexOf("[test]") != -1 );
            if (h(this).indexOf("[警告]") != -1 ) {
                // 警告
                return "<blockquote " + f(this) + " class='blockquote-jinggao' >" + h(this) + "</blockquote>";
            } else if (h(this).indexOf("[推荐]") != -1 ) {
                // 推荐
                return "<blockquote " + f(this) + " class='blockquote-tuijian' >" + h(this) + "</blockquote>";
            } else if (h(this).indexOf("[危险]") != -1 ) {
                // 危险
                return "<blockquote " + f(this) + " class='blockquote-weixian' >" + h(this) + "</blockquote>";
            } else { // info 默认格式
                return "<blockquote " + f(this) + " >" + h(this) + "</blockquote>";
            }
        ```

        

   2. 修改导出时的渲染：通过工具查询 `case a.blockquote:`，这个js文件中有两句，主要是修改下面这句

      - 原来为：

        ```javascript
        case a.blockquote:
        	return "<blockquote class='test'>" + T(e, n) + "</blockquote>";
        ```

        

      - 修改为：如果不想设置为==\[警告\]\[推荐\]\[危险\]==，则需要在下面代码中自己修改

        ```javascript
        case a.blockquote:
            // 转换开始
            if (T(e, n).indexOf("[警告]") != -1 ) {
                // 警告
                return "<blockquote class='blockquote-jinggao'>" + T(e, n) + "</blockquote>";
            } else if (T(e, n).indexOf("[推荐]") != -1 ) {
                // 推荐
                return "<blockquote class='blockquote-tuijian'>" + T(e, n) + "</blockquote>";
            } else if (T(e, n).indexOf("[危险]") != -1 ) {
                // 危险
                return "<blockquote class='blockquote-weixian'>" + T(e, n) + "</blockquote>";
            } else { // info 默认格式
                return "<blockquote class='test'>" + T(e, n) + "</blockquote>";
            }
        ```

   

   

   

   

