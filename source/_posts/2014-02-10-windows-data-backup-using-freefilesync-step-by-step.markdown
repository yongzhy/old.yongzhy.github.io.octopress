---
layout: post
title: "Windows Data Backup Using FreeFileSync Step By Step"
date: 2014-02-10 17:03:53 +0800
comments: true
categories: applications
author: Zhu Yong
---

This article is the step by step guide that I prepared for my colleagues to do the data backup to our newly bought server. I was assigned the task to evaluate some backup tools and find the best one that fits our requirements. After some testing, I finally choose FreeFileSync. I choose it over other tools because it has a feature I like most:

I can mirror folders to backup server, so the backup server is always the latest copy, when I restore, I just copy this folder back to its location; while for those modified / deleted files, they will be saved in another dedicated loation with Date as version. 

OK. Here begins the step by step guide.

---

### Introduction

`FreeFileSync` is a free Open Source software that helps you synchronize files and synchronize folders for Windows, Linux and Mac OS X. It is designed to save your time setting up and running backup jobs while having nice visual feedback along the way.

`FreeFileSync` latest vesrion is 6.2. For more information about this software, can visit its [homepage](http://freefilesync.sourceforge.net)

### Download & Install FreeFileSync

Visit [here](http://freefilesync.sourceforge.net/download.php) to download the latest version of `FreeFileSync`

After download finished, run the downloaded exe file to start setup. You can choose normal installation or portable installation. 

<!-- more -->

### Setup FreeFileSync For Data Backup

The backup scheme that we doing to define should include:

* Daily backup data of selected folders.
* Keep the origin folder structure.
* Keep deleted file for up to 30 days.
* Keep older copy when file been modified.
* No compression on backup file/folder.
* Log file for backup status.

#### Global Setting

From menu, select **Tools**, then **Global Settings**. Maily to set `Retry count` and `Delay`. Recommended setting as shown below:

{% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_globalsetting.jpg "FreeFileSync Setup" %}

#### Backup Setting

{% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_folders.jpg "FreeFileSync Setup" %}

1. Use {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_add_button.jpg "Add Button" %} to add folder pair or {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_del_button.jpg "Delete Button" %} to delete folder pair.

2. Use {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_filter_button.jpg "Local Filter" %} to set what file to include or exclude. Example screen as shown below: 

    {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_filter.jpg  "Filter Screen" %}

3. use {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_sync_setting_button.jpg "Local Sync Setting" %} to set synchronization setting for that folder pair. Example setting screen as shown below. 

    {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_sync_setting.jpg "Filter Screen" %}

    Recommened setting:

    Sync Type : `Mirror` <br/>
    Deleted Files : `Versioning`<br />
    Deleted File Name Convension : `Replace` <br />
    Deleted File Location : `\\\\backup\\location\\**\_old\_\\Path\\to\\backup\\folder\\%timestamp%**`

4. After done folder pair setting. Click {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_batch_button.jpg "Save Batch" %} to save whole backup as batch job. Set the log file location and set a limit on the log entries. Then `Save as` file with extension `.ffs_batch`

    {% img /images/blogimg/2014-02-10-FreeFileSync/FreeFileSync_batchjob.jpg "Batch Job" %}

5. Schedule the batch job. Follow the next section to schedule task step by step.

6. (Optional) Schedule another task to delete backup folder/file that older than 30 days. A script is required, I haven't finished this script yet. When I finish it, I will update this blog to post the script.

#### Schedule Backup Task

Press `Win` + `R` key, then run `taskschd.msc` to start `Windows 7 Task Scheduler`. Then follow images below to schedule the backup step by step. 

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_001.jpg "Task Schedule Step 1" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_002.jpg "Task Schedule Step 2" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_003.jpg "Task Schedule Step 3" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_004.jpg "Task Schedule Step 4" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_005.jpg "Task Schedule Step 5" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_006.jpg "Task Schedule Step 6" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_007.jpg "Task Schedule Step 7" %}

 {% img /images/blogimg/2014-02-10-FreeFileSync/TaskScheduler_008.jpg "Task Schedule Step 8" %}

