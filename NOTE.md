### 参考

看了就会的Next.js SSR/SSG实战教程 - 掘金
https://juejin.cn/post/7133395475675217933


### 以SSR模式运行项目
执行以下命令，build项目：
```
yarn build
```

执行后，在项目根目录下会生成一个.next的目录。这个目录就是用于运行SSR的代码，仅能运行在服务端，不能被浏览器直接运行。

然后再执行以下命令，以SSR模式运行项目：
```
yarn start
```

※注： 每次代码更新，在执行yarn start之前，一定要先执行yarn build。否则运行的并不是最新build的项目。

现在打开http://localhost:3000，看到是SSR模式运行的项目。打开调试工具，看到_document.js设置的代码已生效：

yarn start默认是运行在3000端口，如果想运行在4000端口，可以执行以下命令：
```
yarn start -p 4000
```

更多Next.js CLI命令，可参阅官方说明。
```
Next.js CLI官方说明：https://nextjs.org/docs/api-reference/cli
```


### 引用图片
next自带的<Image>可以认为是<img>的升级版，提供了非常方便的尺寸适配、加载等属性。但也会自动增加很多样式，会影响原生的<img>样式。

所以要根据情况选用。如果只是简单引入图片，无需使用<Image>。

非常建议为<Image>包裹一个父容器，并为父容器定义样式。<Image>会自动适配父容器的大小，因此可以不用为<Image>特意设置width和height。

如果直接使用<Image>，其自动生成的<sapn>容器自带很多样式，很容易导致页面样式错乱。此时，在dev和SSR模式下是可以正常运行的，但在SSG静态化 （export，下一章节介绍）的过程中会报错。

由于<Image>自带的各种优化功能AP1是用于SSR的，是根据客户端的情況（设备类型、屏幕尺寸等）进行图片的动态优化处理，因此无法在SSG中使用（后续章节会讲到SSG)。

解决办法如下，修改next.config.js:
```
  const nextConfig = {
+      images: {
+        unoptimized: true,
+      }
  }
```

next export中关于Image的官方说明：
https://nextjs.org/docs/messages/export-image-api

关闭图片自动优化的官方说明：
https://nextjs.org/docs/api-reference/next/image#unoptimized
