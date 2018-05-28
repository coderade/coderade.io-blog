+++
date = "2018-05-25T13:47:08+02:00"
tags = ["aws", "nodejs", "js"]
categories = ["aws", "nodejs"]
title = "Launching EC2 instances using the Nodejs AWS SDK"
description = "Advanced methods for creating Elastic Cloud Compute instances using the NodeJs AWS SDK."
meta_image = ""
+++

The EC2 service is one of the most fundamental services offered by AWS.
It underpins many of the other services in AWS, so it's always a good idea
understand how to use and the best way to use it. In this post I'll show how to
create EC2 instances directly in the code using the
[AWS SDK for JavaScript in Node.js](https://aws.amazon.com/sdk-for-node-js).

In order to run your application in AWS, we need to have one or more EC2 instances
to execute that code. So the first step is to create an EC2 instance.
There are many different ways to do this.

One of theses ways is use the AWS Console to create instances. I think that is
the easy way, because the [AWS Console](https://aws.amazon.com/console/) is very
interactive and the AWS documentation is very good and easy to understand.
To check how to create instance using the AWS console use the official [Step to step documentation](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html).

Another method is to use the [AWS CLI](https://aws.amazon.com/cli/), that is a
little more complicated, but that have a good documentation too. This  way
can be useful with DevOps automation.

For this post I'll show how to use the SDK to create an instance with code.

But before we can create the instance, we need to create a few other resources.

The first thing that we need to do is create a security group for the instance.
So let's to do this.

## Creating an EC2 Security Group
With a [Node.js project](https://nodejs.org/en/docs/guides/getting-started-guide/)
created the first thing that we need to do is download the AWS SDK module from
the NPM repository. To do
this, use the following command on the root of your project:

```ruby
npm i aws-sdk
```

Then, we will need to create our script file (this will be where we will the
start file for this example) in my example I called it as `create-ec2-instance.js`,
but you can use the name you want.

After that we need to do is to import the AWS SDK into a local variable in this
file. We will create a const declaration with an identifier of AWS all in caps.
Assign to it a require function call will be needed, passing in the string
`'aws-sdk'`.

```javascript
const AWS = require('aws-sdk');
```

The AWS SDK requires you to configure the region each time it's imported. I'll admit this gets a little annoying. There are some abstractions you can do to mitigate this, like set the
region in the AWS config file on your local system or export as environment
variable before running. To understand how to do this use the AWS Docs link of how to
[Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)

For this example, I will call the update function on the `AWS.config` property.
Into this function, pass an object with one property called region, you also could
add other properties to this object to change other configuration options on the
`AWS.config` object, such as the max number of retries for a given request or a
global logger object.
Here just enter a string as the value to the `region` key property with the region
you are using. I'll enter `us-east-1` here for mine.

```javascript
AWS.config.update({region: 'us-east-1'});
```

In my situation, I use more than a region and profile in my computer (for personal
and working coding) so I also need to set my profile using the
`AWS.config.credentials` property. This configuration will get my AWS credentials
data in a shared file used by SDKs and the command line interface on my computer.
To understand better how to set on this way  use the AWS Docs link of how to
[Loading Credentials in Node.js from the Shared Credentials File](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html)

In this example I will load my personal profile using the
`AWS.SharedIniFileCredentials` function and load to the `AWS.config.credentials`
property.

```javascript
let credentials = new AWS.SharedIniFileCredentials({profile: 'personal'});
AWS.config.credentials = credentials;
```
Nice! now we have the Aws settings setted! But, and now?

Now we need an EC2 object created from the AWS object, On this object we'll call
the EC2 operations on.

So, to use it, declare a new `const` variable called `ec2`, all lowercase.
To the right side of an assignment operator, use the new keyword, and call the
[AWS.EC2](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html) class
with no arguments.

There are many other arguments that can be passed on the class constructor like
the `api-version`, `accessKeyId` and `secretAccessKey`, and a `region`, the last
one can be useful if you are using other regions in your code.

To check all the params available to be used on the EC2 constructor, you can   
read the docs for the [AWS.EC2](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html)
class.

But for this example a empty EC2 object is all that's needed to create this object.

```javascript
const ec2 = new AWS.EC2();
```
Now we can start to create the `createSecurityGroup` function.
Most of the AWS SDK operations have a similar pattern. First, we need to declare
the parameters object to be passed to the SDK function. Then call the function,
passing in a callback to handle the result.

For this function we need to create a const named `params` and assign to it an
empty object. We'll going to be using the `createSecurityGroup` function here and
two of the params arguments that it wants is a description and a group name.

This function willl be taking an argument called `sgName` to the name and the
`sgDescription` to the description of the Security Group. Personally  I find that the description isn't too useful until you have a massive number of security groups, even then, it seems that tags
are used more for management in those situations than descriptions.

```javascript
function createSecurityGroup(sgName, sgDescription) {
    const params = {
        Description: sgDescription,
        GroupName: sgName,
    };
}
```

With the params defined, you can continue to the meat of the function. For these
type of functions I personally like to use promises instead of callbacks. IMHO I
think nowadays we must use the most of the advantages of ES6 brings to us and
this approach a little easier to understand and read.

So return a new Promise. It will take a function with the resolve and reject arguments.
Now inside this promise argument function body, we'll call our first SDK function
[ec2.createSecurityGroup()](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createSecurityGroup-property).

The first argument will be the params object we created, then a callback function
with the `err` and `data` arguments. Unfortunately, the SDK doesn't support
promises firsthand, so we'll need to use callbacks here.

Basically, this function is creating a security group with the name and description
that we pass it. It will not create any security group rules. Inside the function body,
call `reject()` with the `err` object if there is one.
In the else, create a block. This is where our code will execute if the security group creation was successful.

```javascript
return new Promise((resolve, reject) => {
    ec2.createSecurityGroup(params, (err, data) => {
        if (err)
            reject(err);
        else {
        }
    })
})
```

Because our security group is essentially empty, we want to add a rule here.
We'll want to enable port 22 so that we can SSH into the instance once it's
been created and port 3000 so if want to run a endpoint example like
[Express](https://expressjs.com/)
on the instance. So start by creating a new `let` called `params` again
and assigning an empty object to it.

There are a few properties required to create this security group rule, which
should sound familiar to you if you've set one up with the console before.

The first is the `GroupId` to apply the rule to. We can get that from the data
argument supplied by the creation callback. For the value here, enter `data.GroupId`.
The next property is `IpPermissions`, which is an array. This array enables us
to configure multiple rules in one request.

```javascript
let params = {
    GroupId: data.GroupId,
    IpPermissions: []
};
```
Then, we need to create an object in this array and add the property `IpProtocol`.

The possible values for this are protocols like `tcp` or `udp`, for some examples.
We will use the tcp here, all lowercase. The next two properties `FromPort `and
`ToPort` define the port range that you're enabling the rule for.

In the first case, we'll enter the number `22` in both `FromPort` and `ToPort`
since I want to allow SSH on this example.

```javascript
let params = {
    GroupId: data.GroupId,
    IpPermissions: [
        {
            IpProtocol: 'tcp',
            FromPort: 22,
            ToPort: 22,
        }
    ]
}      
```

Finally, add the property `IpRanges`
with a value as an empty array. Here's where we can define who this rule applies to.

Add an empty object with the property `CidrIp`. We'll enable access to anyone,
so give this property the value of `0.0.0.0/0`.

```javascript
IpRanges: [
    {
        CidrIp: '0.0.0.0/0'
    }
]    
```


Now we need to also add the rule for port 3000, so copy thes rule object created:

```javascript
{
    IpProtocol: 'tcp',
    FromPort: 22,
    ToPort: 22,
    IpRanges: [
        {
            CidrIp: '0.0.0.0/0'
        }
    ]
}
```

add a comma after it, and then paste. The only different property will be the
`FromPort` and `ToPort`, and you can set both to `3000`.

Awesome! Now we're ready to use this big params object in an SDK call:

```javascript
let params = {
    GroupId: data.GroupId,
    IpPermissions: [
        {
            IpProtocol: 'tcp',
            FromPort: 22,
            ToPort: 22,
            IpRanges: [
                {
                    CidrIp: '0.0.0.0/0'
                }
            ]
        },
        {
            IpProtocol: 'tcp',
            FromPort: 3000,
            ToPort: 3000,
            IpRanges: [
                {
                    CidrIp: '0.0.0.0/0'
                }
            ]
        }
    ]
};
```

On the EC2 object, we'll be calling the `ec2.authorizeSecurityGroupIngress()`
function. The ingress part of that function call refers to these rules being
inbound rules. There is also a function called `authorizeSecurityGroupEgress()`,
which is for outgoing rules, but is not needed on this example.

Call the function and pass the params object as the first argument.
The second will again be a callback function with the argument `err`.

Even though this function does return a second argument, we'll just ignore it.
Inside the callback, call `reject()` again if there's an `err` object.

In the else, just call `resolve()`, and with that code complete, we've successfully
created and assigned rules to a security group.

This is now your `createSecurityGroup` function:

```javascript
function createSecurityGroup(sgName, sgDescription) {
    const params = {
        Description: sgName,
        GroupName: sgDescription,
    };

    return new Promise((resolve, reject) => {
        ec2.createSecurityGroup(params, (err, data) => {
            if (err)
                reject(err);
            else {
                let params = {
                    GroupId: data.GroupId,
                    IpPermissions: [
                        {
                            IpProtocol: 'tcp',
                            FromPort: 22,
                            ToPort: 22,
                            IpRanges: [
                                {
                                    CidrIp: '0.0.0.0/0'
                                }
                            ]
                        },
                        {
                            IpProtocol: 'tcp',
                            FromPort: 3000,
                            ToPort: 3000,
                            IpRanges: [
                                {
                                    CidrIp: '0.0.0.0/0'
                                }
                            ]
                        }
                    ]
                };
                ec2.authorizeSecurityGroupIngress(params, (err) => {
                    if (err)
                        reject(err);
                    else
                        resolve();
                });
            }
        })
    })
}
```
## Creating a key pair
---
