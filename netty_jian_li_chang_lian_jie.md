# netty 建立长连接

###开启服务



```
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try {
            final PushMessageEncodeHander pushMessageEncodeHander = new PushMessageEncodeHander();
            final AuthHandlerAdapter authHandlerAdapter = new AuthHandlerAdapter();

            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                    .channel(NioServerSocketChannel.class)
                    .childHandler(new ChannelInitializer<SocketChannel>() {

                        @Override
                        public void initChannel(SocketChannel ch) throws Exception {
                            // PushMessage转byte，必须放在first，这个在服务端发送消息到客户端时候编码是调用

                            ch.pipeline().addFirst(pushMessageEncodeHander);

                            // byte转成PushMessage  接受消息时候触发使用
                            ch.pipeline().addLast(new PushMessageDecodeHandler());

                            //auth ，处理 及其他逻辑处理
                            ch.pipeline().addLast(authHandlerAdapter);

                        }

                    }).option(ChannelOption.SO_BACKLOG, 10000)
                    .childOption(ChannelOption.SO_KEEPALIVE, true);

            DispatchPushServer.start();

            // Bind and start to accept incoming connections.
            ChannelFuture f = b.bind(port).sync();

            logger.info("===start service address={} ===", f.channel().localAddress());

            // Wait until the server socket is closed.
            // In this example, this does not happen, but you can do that to gracefully
            // shut down your server.
            f.channel().closeFuture().sync();
```

所有的处理就在3个adapter里面完成




