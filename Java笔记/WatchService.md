# WatchService实现监听

```java
public class Watch{
  private ExecutorService threadPool = Executors.newFixedThreadPool(1); 
  
  public static void main(String args[]){
    	for (int i = 0; i < listenThreadSize; i++){
              threadPool.execute(new Runnable() {
                  @Override
                  public void run() {
                      startListen(spiderListenPath);
                  }
              });
          }
    /**
     * 单独一个线程，实现spider config文件的监听和自动reload
     * @param path
     */
    private void startListen(String path) {
        try {
            //监听文件
            WatchService watchService = FileSystems.getDefault().newWatchService();
            Path p = Paths.get(path);
            p.register(watchService, StandardWatchEventKinds.ENTRY_MODIFY,
                    StandardWatchEventKinds.ENTRY_DELETE,
                    StandardWatchEventKinds.ENTRY_CREATE);
            //take watchService 文件变动信号，做相应的处理
            try {
                while (true) {
                    WatchKey watchKey = watchService.take();
                    List<WatchEvent<?>> watchEvents = watchKey.pollEvents();
                    for (WatchEvent<?> event : watchEvents) {
                        //TODO 根据事件类型采取不同的操作
                        String fileName = event.context().toString();
                        if (fileName.equals(spiderListenFileName)) {
                            //如果监测到配置文件有变动，reload config
                            configReloader.reload();
                        }
                    }
                    watchKey.reset();
                }
            } catch (InterruptedException e) {
                logger.error("watchService process error : ", e.toString());
            } finally {
                try {
                    watchService.close();
                } catch (IOException e) {
                    logger.error("watchService close process error : ", e.toString());
                }
            }
        }catch (Exception e){
            logger.error("listen process error : ", e.toString());
        }
    }
  }
}
```

