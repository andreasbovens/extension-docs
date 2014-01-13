---
layout: default-withsidebar
title: Using the downloads API
author: shwetankdixit
copyright: opera-ccby
---

## Introduction

With the [downloads API](downloads.html), you have the ability to initiate, close, search and monitor downloads. It also gives your extension the ability to open the downloads folder or even the downloaded file itself.


## Manifest specifications

You need to list 'downloads' under permissions field in the manifest to enable the extension to work with the downloads API, like so :

<pre class="prettyprint">{
  ...
  "permissions": ["downloads"],
  ...
}</pre>

## The DownloadItem type

Any downloaded item will be of the [`DownloadItem`](downloads.html#type-DownloadItem) type. This object will contain all pertinent information about the downloaded item such as its filename, URL, start time, the total bytes downloaded till now, and other information such as estimated time when the download will end, its MIME type, whether the download can be resumed or not etc. See the documentation for detailed information on the `DownloadItem` type.


## Starting a new download

You can start a download by using the [`download()`](downloads.html#method-download) function. For example,

<pre class="prettyprint">chrome.downloads.download({url: &quot;http://example.org/example.zip&quot;});
</pre>

Of course, you can introduce more arguments to specify how exactly should the download operate, for example if you want the *Save As* download to appear then you can list it in the options. Also, you can have a callback function, like so:

<pre class="prettyprint">
chrome.downloads.download({
  url: &quot;http://example.org/example.zip&quot;, 
  filename: &#39;download.zip&#39;, 
  saveAs: true}, 
   function(downloadId){
     if (typeof downloadId !== &quot;undefined&quot;){ // If &#39;downloadId&#39; is undefined, then there is an error - so making sure it is not so before proceeding.
       console.log(&#39;Download initiated, id is: &#39;+downloadId);
  }
});</pre>

## Searching existing downloads

You can search existing downloads using the [`search()`](downloads.html#method-search) function. You can list various options to specify your search criteria. For example, to list the last 5 downloads descending order (that is, most recent at the top), we could write the following:

<pre class="prettyprint">
chrome.downloads.search(
	{orderBy: [&#39;-startTime&#39;], limit: 5},
	function (downloadedItem) {
		for (var i =0; i&lt; downloadedItem.length; i++){
			console.log(&quot;URL is: &quot;+downloadedItem[i].filename);
		}
	}
);
</pre>

You can use other criteria like the URL, filename, file size etc. One criteria which is particularly usefull is searching by the ID of the downloaded item, as the ID of the item is needed for executing other functions like [pausing](downloads.html#method-pause), [cancelling](downloads.html#method-cancel) or [resuming](downloads.html#method-resume) downloads. You can also [open the folder ](downloads.html#method-show) or [open the actual downloaded file](downloads.html#method-open) provided you know the download id. 

## Working with events

You also have access to certain events which can be very helpful. In particular, [`onCreated`](downloads.html#event-onCreated) and [`onChanged`](downloads.html#event-onChanged). You can use the `onCreated` event in the background script like so:

<pre class="prettyprint">
chrome.downloads.onCreated.addListener(function (e) {
	console.log(&quot;New Download created. Id:&quot;+e.id+&quot;, URL: &quot;+e.url+&quot;, fileSize:&quot;+e.fileSize);
});
</pre>

You can monitor downloads and work on them whenever their state changes using the `onChanged` event. For example, to detect whenever a download completes, you can write the following in the background script:

<pre class="prettyprint">
chrome.downloads.onChanged.addListener(function (e) {
	if (typeof e.state !== &quot;undefined&quot;) {
		if (e.state.current === &quot;complete&quot; ){
			console.log(&quot;download id&quot;+e.id+&quot; has completed.&quot;);
		}
	}
});
</pre>

[Download our sample extension](samples/DownloadsAPI.nex) which makes use of the API methods and events to show the you last 5 downloaded files as well as information about new downloads. 


