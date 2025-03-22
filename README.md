<h1>Amazon EC2 Auto Scaling (Employee app)</h1>

<h2>Description</h2>
- As more people visit our application, the demand on our two web servers is going to increase, At some point, our two instances aren't going to be able to handle that demand,and we're going to need to add more EC2 instances.
Instead of launching these instances manually, we want to do it automatically with EC2 Auto Scaling.
Autoscaling is what allows us to provision more capacity on demand depending on different thresholds that we set and we can set those in CloudWatch.
These EC2 instances will be part of an auto-scaling group. Each of the EC2 instances that we launch will be completely identical, then we're going to run code to simulate stress on our employee directory application that will make the instances think that they're overloaded, when this happens, the instances will report to CloudWatch and say that their CPUs are overloaded.
CloudWatch will then go into an alarm state and tell auto-scaling, give me more EC2 instances, and as each instance comes online, they will pass ALB Health checks, They'll start to receive traffic
and give us the horizontal scalability that we need.
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742619713/Screenshot_2025-03-22_005301_duyaue.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742619704/Screenshot_2025-03-22_005232_xoxsfk.png"/>
<br />

<h2>Program walk-through:</h2>

1. Create a launch template (we want our instances to be identical and that's what we'll be configuring here)
- Find launch templates on the side panel and click Create a launch template
- Provide a name and description, call this app-launch-template, and give it a description of a web server for the employee directory app.
- Then you'll notice this handy check box that asks if we are going to use this launch template with EC2 Auto Scaling.
- Choose the AMI, again, the launch template is all about what we launch, So what we want to do is create mirrors of the web servers hosting our app that we already have running, that way whenever we scale out, we just have more instances with the exact same configuration.
- When we launched the instance hosting our app earlier, we used the Amazon Linux, AMI, and a T2 micro instance type, so we'll select both of those options.
- Next, we choose the security group for our new instances, We'll use the same security group you created earlier in the course, the web security group.
- Expand the advanced details selection here we'll choose our instance role, the same role we used previously in the instance profile dropdown.
- we'll scroll all the way down and paste our user data ( This is what grabs our source code and unzips it so that our application runs on EC2).
- Now that we've configured our launch template which again defines what we launch
- 
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621069/Screenshot_2025-03-21_192232_yjudn2.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621076/Screenshot_2025-03-21_192338_c6qati.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621080/Screenshot_2025-03-21_192557_o47biz.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621084/Screenshot_2025-03-21_192631_apxlxf.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621089/Screenshot_2025-03-21_192712_ahjc1i.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621093/Screenshot_2025-03-21_192800_n3hn9l.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621117/Screenshot_2025-03-21_193808_vquwk5.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742621121/Screenshot_2025-03-21_193925_hy6bfg.png"/>

<br />

2. We now have to define an auto-scaling group
- To create an autoscaling group, we'll select autoscaling groups on the side panel and then create an autoscaling group.
- we'll enter a name such as app-asg and then select the launch template we just created app launch template and then click next.
-  we'll select our network, We'll choose the same VPC we created earlier in the course, App-vpc, and select both the private subnets we created, Private A and Private B, and then click next.
-  We then need to select attached to an existing load balancer to receive traffic from the load balancer we created earlier and then choose our target group, at Target group.
-  Click enable ELB health checks so that the load balancer will check if your instances are healthy and then click next.
-  we'll choose the maximum minimum and desired capacity, The minimum we'll say is two. This means that our auto-scaling group will always have at least two instances, one in each availability zone.
The maximum will is four, which means that our fleet can scale up to four instances. And the desired capacity, which is the number of instances you want to be running, we'll say is two.
- Next, we can configure the auto-scaling policies, With scaling policies, you define how to scale the capacity of your auto-scaling group in response to changing demand, (For example, you could set up a scaling policy with CloudWatch that whenever your instant CPU utilization or any other metric that you'd like reaches a certain limit, you can deploy more EC2 instances to your auto-scaling group) So what we want to do is use target scaling policies to adjust the capacity of this group.
- we're going to create a target tracking policy, like created the alarm, but this time it will result in the action of triggering an auto-scaling event.
- we'll name this CPU utilization and then we'll say that we want to add a new instance to our fleet whenever the target value is 60%, In addition We'll also keep the instances need setting at 300 seconds
to warm up before scaling.
- we'll click next to configure notifications when a scaling event happens, This is optional, so for now we're going to skip past it, All right, here we can review and click Create autoscaling group.

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623353/Screenshot_2025-03-21_193950_q5gvja.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623360/Screenshot_2025-03-21_194031_tfaheu.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623369/Screenshot_2025-03-21_194520_gxbhah.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623376/Screenshot_2025-03-21_194638_nwsgjp.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623473/Screenshot_2025-03-21_194730_or9m0v.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623480/Screenshot_2025-03-21_194822_d6zz4h.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623489/Screenshot_2025-03-21_195933_umwqm2.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623512/Screenshot_2025-03-21_195955_bjeycy.png"/>

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742623697/Screenshot_2025-03-21_200007_rmyngz.png"/>

<br />

3. Now all that's left is to stress our application and make sure that it scales up to meet the demand. To do that, I'll open up a new tab and paste the endpoint for our elastic load balancer.
-  on the  application. I'm going to go to the info page by app pending/info on the URL.
-  You'll notice here that we built in a stress CPU feature. this is going to mimic the load coming into our servers, you would probably use a load-testing tool, but instead, we built a stress CPU button
as a quick and easy way to test this out, then we can watch our servers scale with that auto-scaling policy.
- So as the CPU utilization goes up, our auto-scaling policy will be triggered and our server groups will grow in size.
- I'm going to select 10 minutes for our stress test after some time has passed, We've stressed our application, and if we take a look at CloudWatch ,we can see what happened.
- let's click on CloudWatch. If you look here, we can see our alarm summary, We went over 60% CPU utilization across instances.
- That was our threshold, so we launched new instances, We launched two new instances into our autoscaling group.
- Then you can see the loads start to come down because we've launched more instances into our autoscaling group, there were more hosts to accept traffic for our elastic load balancer and the average CPU utilization went down across servers.

<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742663048/Screenshot_2025-03-21_200031_a6dgqd.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742663072/Screenshot_2025-03-21_200110_wdebdm.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742663086/Screenshot_2025-03-21_200124_bjfrig.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742663095/Screenshot_2025-03-21_200159_w4xwnm.png"/>
<img src="https://res.cloudinary.com/dk3bkl3ji/image/upload/v1742663106/Screenshot_2025-03-21_200215_sfa5u1.png"/>
<br />
