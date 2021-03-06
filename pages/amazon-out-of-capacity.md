# Boot after an Amazon Out of Capacity Message

<html>
<iframe src="http://player.vimeo.com/video/16323186?title=0&amp;byline=0&amp;portrait=0&amp;color=ffffff" width="600" height="375" frameborder="0"></iframe>
</html>

**NOTE: Although we offer these instructions to help you quickly solve this issue manually, we are working on an automated solution.**

When you are booting up your instance sometimes you will run into this message: "Amazon is currently out of capacity".

* You will need to wait for about a minute and then you will see an **Acknowledge** link right next to the **Custom log** link appear. If you have an application master and master database running the link will appear for each of them.

* Once the links appear click on them until you have no running instances in your environment. Then you will want to click on the **More Options** tab.

* In the more options section you will see many options to choose from. For this particular case you will want to click on the **Edit Environment** link.

* You will then be presented with the **Edit the Environment** page that you had during the quick start process. Here you will want to click on the **(show advanced options)** link.

* Now you will be presented with a list of advanced option that includes changing the Ruby runtime and the most important for this error, the **Availability Zone**. Change the availability zone from what it currently is to another area. Once you have done this make sure to click the **Update Instances** button.

* You will then be redirected to the dashboard. Here you will want to click on the **Boot Instance** button. Then choose what type of configuration you want to have and boot up again.

When you boot up again the error should disappear and your application and master server will be setup and ready to go.  Repeat (if at all necessary).