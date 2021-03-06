Downloader
==========

A library for resuming and multi-part/multi-threaded downloads in .NET written in C#

The library uses .NET 3.5 and threads to support all platforms from Windows Vista on.

Example for usage:
Start a resuming download with 4 parts of this README.md to local file README.md.

```C#
var url = new Uri("https://raw.githubusercontent.com/Toqe/Downloader/master/README.md");
var file = new System.IO.FileInfo("README.md");
var requestBuilder = new SimpleWebRequestBuilder();
var dlChecker = new DownloadChecker();
var httpDlBuilder = new SimpleDownloadBuilder(requestBuilder, dlChecker);
var timeForHeartbeat = 3000;
var timeToRetry = 5000;
var maxRetries = 5;
var rdlBuilder = new ResumingDownloadBuilder(timeForHeartbeat, timeToRetry, maxRetries, httpDlBuilder);
List<DownloadRange> alreadyDownloadedRanges = null;
var bufferSize = 4096;
var numberOfParts = 4;
var download = new MultiPartDownload(url, bufferSize, numberOfParts, rdlBuilder, requestBuilder, dlChecker, alreadyDownloadedRanges);
download.DownloadCompleted += (args) => Console.WriteLine("download has finished!");
var dlSaver = new DownloadToFileSaver(file);
dlSaver.Attach(download);
download.Start();
```

For a more sophisticated example also demonstrating the download observers functionality, please have a look at the Downloader.Example project.
