# **Extract a big tar.gz file quickly**

The solution was *pigz*. This is an advanced version of *gzip*. It uses multiple threads for reading, writing and checksum calculations.

If you want to see the progress of the extraction process, you need to use Pipe Viewer (pv). PV (“Pipe Viewer”) is a tool for monitoring the progress of data through a pipeline. It can be inserted into any normal pipeline between two processes to give a visual indication of how quickly data is passing through, how long it has taken, how near to completion it is, and an estimate of how long it will be until completion.

```sh
sudo pacman -S pigz pv
pigz -dc <FILE>.tar.gz | pv | tar xf -
```
