## Rail搭建博客项目报告

### 项目任务：
1. 根据 [Rails guides](http://guides.rubyonrails.org/getting_started.html)，写一个简单的博客网站
2. 使用 [Bootstrap](http://getbootstrap.com/) 作为此博客网站的前端框架，应用 Bootstrap 的基本元素，使得网站美观。
3. 部署网站到任何一个云服务器（国内国外都可以，如果没有自己的服务器，可以使用[heroku](https://www.heroku.com/home)

### 项目完成情况:
1. 按照Rail Guides写成了一个简单的博客网站，包含article的CRUD操作和comment的CD操作
2. 使用Bootstrp对网站进行了基本的美化
3. 将网站部署到了[heroku](https://hidden-bastion-64624.herokuapp.com/)上

### 项目过程中遇到的问题:

1. 博客后台的搭建（mvc模式开发）：
后台的搭建基本按照教程，每一步都有详细的指导，所以没有遇到太多问题。

2. bootstrap美化：
2.1 在rail中引入bootstrap
通过网络检索，发现了比较标准的方法是通过bootstrap-sass向rails项目中引入。为什么需要该第三方项目呢？因为bootstrap支持less，而rails只支持sass，所以需要这个项目来把我们sass写的代码转成bootstrap能看懂的格式。
引入过程在项目的[github文档](https://github.com/twbs/bootstrap-sass)中有详细说明。
2.2 bootstrap很多color不能生效
在美化过程中，想使用[doc](https://getbootstrap.com/docs/4.0/utilities/colors/)里面提到的一些很好看的文字颜色都没法生效，类似的还有背景的颜色和button的颜色。
暂时没有找到原因，所以先选用了一些能使用且美观程度还不错的颜色和图标。
2.3 表单的美化
rails有自带的表单生成格式(form_with)，表单的各个部分都通过erb写成。使用bootstrap调整起来比较麻烦，所以只是简单的使用form_with自带的功能调整了文本框的大小，用bootstrap调整了位置和按钮的格式，没有做太多的优化。

3. heroku部署：
根据heroku官网的[教程](https://devcenter.heroku.com/articles/getting-started-with-rails5#database)完成部署。唯一需要调整的是heroku的服务器不支持sqlite3，需要换成postresql。所以本地的gemfile将sqlite换成pq，另外database的adapter也需要调整，在config/database.yml里面。
