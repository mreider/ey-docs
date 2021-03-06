# AppCloud Instances

Currently Engine Yard AppCloud is built on top of Amazon's AWS services. When you create an instance with Engine Yard you're booting up an [[EC2|http://aws.amazon.com/ec2/]] instance. We boot this instance for you and automatically configure it with the needed [[Engine Yard Stack|http://www.engineyard.com/products/technology/stack]] components for your environment.

There are various roles that an instance can play in your application's environment. These include **Application Instances**, **Database Instances** and **Utility Instances**. The only time we'll combine multiple roles into a single instance is when you boot up a **single instance environment**. 

Both your database and your application's code are written out to **persistent storage** volumes. We automatically mount these volumes and take backups for you. 

When booting up new instances there are various **instance sizes** you can choose from to help meet your specific needs.

## Application Instances

These instances are configured to run your ruby based application. They're configured with the Engine Yard tried-and-true stack. Your application will automatically be deployed onto the `/data` mount which is persistent storage.

We backup `/data` volume only on the **application master** instance. 

You can run [[crons|adding-cron-jobs]] on your Application instances and you can setup crons to run on the application master instance via the web UI's Crontab page. If your crons are CPU or Memory intensive you might consider off loading the work to a Utility Instance.

## Database Instance

This instance is configured to run your database. Running your database on a separate server prevents your database and application from contending for the same resources. 

MySQL is currently the only supported database on Engine Yard AppCloud. If you need to run a postgres DB you'll have to set it up via a custom chef recipe. Here's a template to get you started: http://github.com/engineyard/ey-cloud-recipes/tree/master/cookbooks/postgres/

Your database resides on the `/db` mount. This mount point is persistent and can be used to restore your database later. We take regular snapshots and backups of your database by default.

## Utility Instances

Utility instances are great for off loading any heavy work from your application instances. It's common to run background jobs, job queues, and crons on utility instances. They are also useful for running large memcached instances. 

We automatically push your code to the utility instances with each deploy for you. This allows your crons and background jobs to access any application code that they need.

## Single Instance Environment

Single instance environments, commonly called a solo instance, are great for staging and development environments since they'll save you money. You can boot up just a single instance and we'll deploy both your application and your database to this instance. These types of environments are not, however, ideal for product because your database and application will be contending for the same system resources. 

If you boot up a single instance environment to start with you can upgrade it to a multi-instance environment later.

## Persistent Storage

Both your `/data` mount on the app master and `/db` mount on the database master instance(s) are persistent. We take advantage of Amazon's EBS storage allowing us to take regular disk snapshots of both of these volumes. If you ever have to rebuild your instances from scratch you will have the ability to restore both of these volumes from previous snapshots. 

## Instance Sizes

All the way from the small to the extremely large, you have 7 instance size broken into 3 categories to choose from. You can pick the proper instance size to fix you specific needs.

<html><span style="color:#999900;">The <strong>High CPU Medium</strong></span></html> instances are a top choice for your application when you start taking on real production traffic.

Normal instances are a well balanced match of CPU and RAM.
  
  * **Small** is 32-bit with 1.7 GB RAM, 1 ECU
    * These are great for staging and development environments. However, with only 1 ECU they're not ideal for environments taking real production traffic. When these types of instances taken on some traffic they tend to become CPU starved. 
  * **Large** is 64-bit with 7.5 GB RAM, 4 ECU
    * These instances are great when your application or database needs extra RAM but is not CPU starved.
  * **Extra Large** is 64-bit with 15 GB RAM, 8 ECU
    * These instances pack extra RAM and CPU but are still affordable for those who need it.

High Memory instances come with extra RAM and are ideal when you need really need a large amount of RAM.
  
  * **High Memory Double Extra Large** is 64-bit with 34.2 GB RAM
    * These instances are a perfect fit for databases with large datasets. It allows you to load much of your database into RAM helping relieve disk reads from your database instance.
  * **High Memory Quadruple Extra Large** is 64-bit with 68.4 GB RAM, 26 ECU
    * These are the largest instances you can boot up. Their size speaks for themselves.

High CPU instances come with extra CPU and are ideal when you need extra processing power.
  
  * <html><span style="color:#999900;"><strong>High CPU Medium</strong></span></html> is 32-bit with 1.7 GB RAM, 5 ECU
    * These are the most ideal instance size when your app is first starting to take on real production traffic. They're also a great size for scaling out horizontally, allowing you to add or remove application instances as need at an affordable rate. The 5 ECU's, equating to 2 VCPUs, is a wonderful balance of RAM to CPU. Give these a try, we're sure you'll love them.
  * **High CPU Extra Large** is 64-bit with 7 GB RAM, 20 ECU
    * This instance size is great for those applications that are very CPU intensive.
