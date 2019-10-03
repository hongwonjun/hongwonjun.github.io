---
layout: post
title: "Redmine - file attachment Error"
date: 2015-09-03 00:00:00 +0900
author: Hong Wonjun
categories: Trouble-Shooting
tags: redmine
---

Error Log.

{% highlight clike %}
Started POST "/uploads.js?attachment_id=******.docx&amp;content_type=application%2Fvnd.openxmlformats-officedocument.wordprocessingml.document" for ***.***.***.*** at ****-**-** **:**:** +****
Processing by AttachmentsController#upload as JS
  Parameters: {"attachment_id"=&gt;"******.", "filename"=&gt;"******.docx", "content_type"=&gt;"application/vnd.openxmlformats-officedocument.wordprocessingml.document"}
  Current user: ****** (id=*)
Saving attachment '******/redmine/files/****/**/*************_****************.docx' (******* bytes)
Completed 500 Internal Server Error in 12ms (ActiveRecord: 0.6ms)

Errno::EACCES (Permission denied - ********/redmine/files/****/**/*************_****************.docx):
  app/models/attachment.rb:109:in `initialize'
  app/models/attachment.rb:109:in `open'
  app/models/attachment.rb:109:in `files_to_final_location'
  app/controllers/attachments_controller.rb:90:in `upload'
  lib/redmine/sudo_mode.rb:63:in `sudo_mode'
{% endhighlight %}

이런 경우 보통 permission 값 때문인데, files 는 이미 웹서버 권한으로 설정이 되어 있는 상태이다.

확인결과,  
기존에 migration 을 시행할 때 첨부파일 때문에 files 하위에 root 권한으로 폴더가 생성되어 있었다.

{% highlight clike %}
$ sudo chown -R www-data:www-data files/
{% endhighlight %}

redmine 설치 위치에서 위의 명령어를 실행하면 된다.