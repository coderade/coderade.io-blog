+++
date = "2018-06-24T13:47:08+02:00"
tags = ["aws", "nodejs", "js", "ec2"]
categories = ["aws", "nodejs"]
title = "Launching EC2 instances using the Node.js AWS SDK"
description = "Advanced methods for creating Elastic Cloud Compute instances using the Node.js AWS SDK."
meta_image = "images/posts/aws/ec2-instances.png"
+++

The EC2 service is one of the most fundamental services offered by AWS. 
It underpins many of the other services in AWS, so it’s always a good idea 
to understand how to use and the best way to use it.

In this post, I’ll show how to create EC2 instances directly in the code using the
[AWS SDK for JavaScript in Node.js](https://aws.amazon.com/sdk-for-node-js).

In order to run your application in AWS, we need to have one or more EC2 instances to execute that code. 
So, the first step is to create an EC2 instance, there are many different ways to do this.

One of these ways is to use the AWS Console to create instances. 
I think that is the easy way because the [AWS Console](https://aws.amazon.com/console/)
is very interactive and the AWS documentation is very good and easy to understand. 
To check how to create an instance using the AWS console using the official
[Step to step documentation](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html).

Another method is to use the [AWS CLI](https://aws.amazon.com/cli/), which is a little more complicated, 
but that have good documentation too. This way can be useful with DevOps automation.

For this post, I’ll show how to use the SDK to create an instance with code.

But before we can create an instance, we need to create a few other resources.

In this post, I will show how created the needed resources to use an EC2 instance.

The first thing that we need to do is create a security group for the instance.

So let’s do this.

## Creating an EC2 Security Group
With a [Node.js project](https://nodejs.org/en/docs/guides/getting-started-guide/)
created, the first thing that we need to do is download the AWS SDK module from the
[NPM](https://www.npmjs.com/) repository.

To do this, use the following command on the root of your project:

```ruby
npm i aws-sdk
```

Then, we will need to create our script file (this will be where we will the
start file for this example), In this example, I called it as `create-ec2-instance.js`,
but you can use the name you want.

After that, we need to import the AWS SDK into a local variable in this
file. We will create a const declaration with an identifier of AWS all in caps and
assign the module named `'aws-sdk'` to it using the require function.


```javascript
const AWS = require('aws-sdk');
```

The AWS SDK requires you to configure the region each time it's imported. I'll
admit this gets a little annoying. There are some abstractions you can do to
mitigate this, like set the region in the AWS config file on your local system or
export as an environment variable before running.

To understand how to do this use the AWS Docs link of how to
[Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)

For this example, I will call the update function on the `AWS.config` property.
Into this function pass an object with one property called `region`, you also could
add other properties to this object to change other configuration options on the
`AWS.config` object, such as the max number of retries for a given request or a
global logger object.

Here just enter a string as the value to the `region` key property with the region
you are using. I'll enter `us-east-1` here for mine.

```javascript
AWS.config.update({region: 'us-east-1'});
```

In my situation, I use more than a region and profile on my computer (for personal
and working coding) so I also need to set my profile using the `AWS.config.credentials`
property. This configuration will get my AWS credentials data in a shared file used by
SDKs and the command line interface on my computer.
To understand better how to set on this way, use the AWS Docs link of how to
[Loading Credentials in Node.js from the Shared Credentials File](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/loading-node-credentials-shared.html)

In this example, I will load my personal profile using the
`AWS.SharedIniFileCredentials` function and load to the `AWS.config.credentials`
property.

```javascript
let credentials = new AWS.SharedIniFileCredentials({profile: 'personal'});
AWS.config.credentials = credentials;
```
Nice! now we have the Aws settings set! But and now?

Now we need an EC2 object created from the AWS object, on this object we'll call
the EC2 operations on.

So, to use it, declare a new `const` variable called `ec2`.
To the right side of an assignment operator, use the new keyword and call the
[AWS.EC2](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html) class
with no arguments.

There are many other arguments that can be passed on the class constructor like
the `api-version`, `accessKeyId` and `secretAccessKey` and a `region`. The last
one can be useful if you are using other regions in your code.

To check all the params available to be used on the EC2 constructor, you can   
read the docs for the [AWS.EC2](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html)
class.

But for this example, an empty EC2 object is all that's needed to create this object.

```javascript
const ec2 = new AWS.EC2();
```
Now we can start to create the `createSecurityGroup` function.
Most of the AWS SDK operations have a similar pattern. First, we need to declare
the parameters object to be passed to the SDK function. Then call the function,
passing in a callback to handle the result.

For this function, we need to create a const named `params` and assign to it an
empty object. We'll use the `createSecurityGroup` function here and
two of the params arguments that it wants are a description and a group name.

This function will be taking an argument called `sgName` for the name and the
`sgDescription` for the description of the Security Group.

Personally, I find that the description isn't too useful until you have a massive
number of security groups, even then, it seems that tags are used more for management
in those situations than descriptions.

```javascript
function createSecurityGroup(sgName, sgDescription) {
    const params = {
      GroupName: sgName,
      Description: sgDescription
    };
}
```

With the `params` defined, we can continue to the creation of the function. For these
types of functions, I personally like to use promises instead of callbacks. IMHO I
think nowadays we must use most of the advantages that ES6 brings to us and
this approach a little easier to understand and read.

In this way, we will return a new Promise. It will take a function with the
resolve and reject callbacks.
Now inside this promise argument function body, we'll call our first SDK function
[ec2.createSecurityGroup()](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createSecurityGroup-property).

The first argument will be the params object which we created, then a callback function
with the `err` and `data` arguments. Unfortunately, the SDK doesn't support
promises firsthand, so we'll need to use callbacks here.

Basically, this function is creating a security group with the name and description
that we pass it. It will not create any security group rules. Inside the function body,
call `reject()` with the `err` object if there is one.
In the else, create a block. This is where our code will execute if the security
group creation was successful.

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
been created and port 3000 so if want to run an endpoint example like
[Express](https://expressjs.com/)
on the instance. So start by creating a new `let` called `params` again
and assigning an empty object to it.

There are a few properties required to create this security group rule, which
should sound familiar to you if you've set one up with the console before.

The first is the `GroupId` to apply the rule to. We can get that from the data
argument supplied by the creation's callback. For the value here, enter `data.GroupId`.
The next property is `IpPermissions`, which is an array. This array enables us
to configure multiple rules in one request.

```javascript
let params = {
    GroupId: data.GroupId,
    IpPermissions: []
};
```
Then, we need to create an object in this array and add the property `IpProtocol`.

The possible values for this are protocols like TCP or UDP, for some examples.
We will use the value `'tcp'` here, all lowercase. The next two properties `FromPort `and
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

Finally, add the property `IpRanges`' value as an empty array. Here's where we can define who this rule applies to.

Add an empty object with the property `CidrIp`. We'll enable access to anyone,
so give this property the value `0.0.0.0/0`.

```javascript
IpRanges: [
    {
        CidrIp: '0.0.0.0/0'
    }
]    
```


Now, we need to also add the rule for port 3000, so we can copy the rule object created:

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

add a comma after it and then paste. The only different property will be the
`FromPort` and `ToPort` and you can set both to `3000`.

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

In the else, just call `resolve()` and with that code complete, we've successfully
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

To log in the instance, we must create a key pair. According the Amazon
[docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
the use for the Key pair is:

> Amazon EC2 uses public–key cryptography to encrypt and decrypt login information.
Public–key cryptography uses a public key to encrypt a piece of data, such as a password,
then the recipient uses the private key to decrypt the data.
The public and private keys are known as a key pair.


In this way we will need to create a Key Pair to connect to our EC2 instance.

So we need a `createKeyPair` function, this function will take a `keyName` argument.

```javascript
function createKeyPair(keyName) {}
```

This function will be a wrapper for the aws-sdk
[ec2.createKeyPair](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createKeyPair-property)
function.

This function has the 'KeyName' and the 'DryRun' arguments, but for this example only
the 'KeyName' argument is needed. The value of this argument will passed in our
function `createKeyPair` function.

```javascript
let params = {
  KeyName: keyName, /* required */
};
```

After declaring the params object, let's return a new Promise with a callback
function with arguments resolve and reject as the argument.

Inside the callback function, we can call the EC2 function createKeyPair,
passing the `params` object as the first argument. The second argument will be a
callback function with the err and data objects.

If there's an error, then call reject with the err object. In the else, call resolve
with the data object.

```javascript
return new Promise((resolve, reject) => {
       ec2.createKeyPair(params, (err, data) => {
           if (err)
               reject(err);
           else
               resolve(data)
       })
   })
```

This will be our `createKeyPair` function:

```javascript
function createKeyPair(keyName) {
    const params = {KeyName: keyName};

    return new Promise((resolve, reject) => {
        ec2.createKeyPair(params, (err, data) => {
            if (err)
                reject(err);
            else
                resolve(data)
        })
    })
}
```

This data object is important because when the key pair is created in AWS,
the function returns the contents of that key pair's private key.

If you don't resolve the data argument on the `ec2.createKeyPair()` function,
it basically throws away the created key pair and if we don't
save this locally, the key pair is essentially useless.

For this way I created a KeyPair helper function that will actually take
this key pair data and save it locally as a file, this function I saved on
the helpers directory with the name `keyPairHelper.js`. This file will contains
the below code:

```javascript
const fs = require('fs');
const path = require('path');
const os = require('os');

function persistKeyPair (keyData) {
    return new Promise((resolve, reject) => {
        const keyPath = path.join(os.homedir(), '.ssh', keyData.KeyName);
        const options = {
            encoding: 'utf8',
            mode: 0o600
        };

        fs.writeFile(keyPath, keyData.KeyMaterial, options, (err) => {
            if (err) reject(err);
            else {
                console.log('Key written to', keyPath);
                resolve(keyData.KeyName)
            }
        })
    })
}

module.exports = {
    persistKeyPair: persistKeyPair
};
```

This function `persistKeyPair()` will be called after the `createKeyPair()` function
which consequently will be called as the return function for our `createSecurityGroup()`
function at the begin of your `create-ec2-instance` file.

```javascript
createSecurityGroup(sgName, sgDescription)
    .then(() => {
        return createKeyPair(keyName);
    })
    .then(keyPairHelper.persistKeyPair)
```

With these methods our key pair will be created.

Now that we have the methods to create the security group and key pair, we can
finally create an EC2 instance using both.


## Creating the EC2 Instance

The only one unimplemented function in our example is the `createInstance()`
function.

This function takes in the `sgName` (security group name) and the `keyName`
(key pair name) as arguments, the same arguments that were used to create the
other resources.


```javascript
function createInstance(sgName, keyName) {}
```

Inside the function, we'll start by defining a `params` object.

This `params` object will have many more arguments than the other function ones
we've made.

The first we'll need to add is `ImageId`. This is the AMI ID that will be used
to create the instance.

To get a `ImageId` we need to select a AMI, so let's get one.

### Selecting a Amazon Machine Image (AMI)

There is an EC2 SDK function called
[describeImages](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeImages-property)
that can be used to search for AMIs that we can create. The problem is the search
takes a long time and it's really just easier to go to the console directly if
you know what you're looking for.

As we only need the ImageId for our AMI, the easiest way is suitable.

So, to select the AMI, we need to go to the [AWS Console](https://console.aws.amazon.com)
and then to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1).

Then, we can click on the *Launch Instance* button and we'll be presented with
the AMI selection screen.

![](/images/posts/aws/ami-selection-screen.png)


At the top, there is the Amazon Linux AMI. At the end of that title is the ID.

Yours may or may not be the same as the above image based on when you're reading
this  post and which region you're using.

Either way, for this example, if you don't need a especific one, you can copy
whatever AMI ID is in your AWS Console.

Now we can switch back to your code and paste that in a string as the value to the
`ImageId` property.

```javascript
ImageId: 'ami-14c5486b', //AMI ID that will be used to create the instance
```
The next property is `InstanceType`. This is the type of instance such as `t2.small`
or `m4.xlarge` and defines the properties of the instance. Like the AMI, if you
don't need a specific one you can use one of the smallest here: **t2.micro**.

```javascript
InstanceType: 't2.micro',
```

Next, we need to define the key pair name with the property `KeyName` and `Name`,
that we can be entering the value of keyName from the function arguments for both.

```javascript
KeyName: keyName,
Name: keyName,
```

Next, is the `MaxCount` and `MinCount` property, which tells AWS how many instances to create.
We can enter 1 for both.

```javascript
MaxCount: 1,
MinCount: 1,
```

For more information about the default limits and how to request an increase,
see [How many instances can I run in Amazon EC2](https://aws.amazon.com/ec2/faqs/#How_many_instances_can_I_run_in_Amazon_EC2)
in the Amazon EC2 FAQ.

Now we need to add the property `SecurityGroups` and give it an array as the value.
This is where we add any security groups to the instance.
So, we enter the `sgName` argument into the array.

```javascript
SecurityGroups: [
            sgName
        ],
```

We can add more than one security group here if you want to.
There's also a [`SecurityGroupIds`](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#runInstances-property)
property that we could use instead and reference the security groups by their IDs
rather than by their names. I find it a little easier to do it by name for this code.

The last property in our params object is `UserData`.

The `UserData` has a couple of different uses with EC2 instances, but we will use it to
run shell commands once the instance starts. To understand more of it, please read the
AWS documentation of how to
[run Commands on Your Linux Instance at Launch](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html).

For this example, I will use the following commands on my `UserData` value:

```javascript
let commandsString = `#!/bin/bash
  curl --silent --location https://rpm.nodesource.com/setup_10.x | sudo bash -
  sudo yum install -y nodejs
  sudo yum install -y git
  git clone https://github.com/coderade/aws-ec2-examples
  cd repo
  npm i
  npm run start`;
```

Essentially, these commands are just installing Node and Git, cloning the
example project from my [GitHub](https://github.com/coderade), installing the
project dependencies and then running it.

You can't just put these shell scripts directly into the `UserData` value, first
we have to Base64 encode it.

To do this, with Node.js we can create a [Buffer](https://nodejs.org/api/buffer.html#buffer_buffer)
of this variable and then convert it to string with the base64 encoding type.

```javascript
UserData: new Buffer(commandsString).toString('base64');
```

Now, this will be our final params variable that will send to the EC2
[runInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#runInstances-property)
method:


```javascript
const params = {
    ImageId: 'ami-14c5486b', //AMI ID that will be used to create the instance
    InstanceType: 't2.micro',
    KeyName: keyName,
    Name: keyName,
    MaxCount: 1,
    MinCount: 1,
    SecurityGroups: [
        sgName
    ],
    UserData: new Buffer(commandsString).toString('base64')
};
```
This is the function that actually does the EC2 instance creation.

Now, we'll return a new Promise with the callback function with resolve and reject
arguments.

Inside that function, we'll call the `ec2.runInstances`, so we will pass `params`
object as the first argument and the second will be a callback function
with `err` and `data` arguments.

```javascript
return new Promise((resolve, reject) => {
        ec2.runInstances(params, (err, data) => {
            if (err)
                reject(err);
            else
                resolve(data)
        })
    })
}
```

Then, we will call `reject` with the `err` argument if it exists, otherwise, we
will call `resolve` with the `data` argument.

We also need to create a method to create our instance tag (that is used as the
name of the instance to help the instances management).

In this way, let's create the `createInstanceTag()` method which receive the
`instanceData` that will be the data resolved by the promise of the `createInstance()`
method and the `instanceTagName` that will be also passed by that promise
resolution.

On this method, we to create a object called `params` that will contains the
`Resources` and the `Tags` properties.

The `Resources` property will be an array that will contains the IDs of one
or more instances resources to be tagged. In our example, we will use the ID of
the instance who we just created:

```javascript
  Resources: [instanceData.Instances[0].InstanceId]
```

The `Tags` property will be an array of that contains one or more tags. For
this, I will create only one tag with the key *Name*.

The tags object contains two String properties. The `Key` that is actually the
key of the tag and the `Value` that as the name says is the value the of the tag.

So, to create our `Name` tag we will set value `'Name'` for the `Key` and the
`instanceTagName` for the `Value` property.

```javascript
Tags: [
    {
        Key: 'Name',
        Value: instanceTagName
    }
]
```

Inside the function, like the another functions we'll call a EC2 method the
[`ec2.createTags`](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createTags-property).
Then, we will pass `params` object as the first argument and the second will
be a callback function with `err` and `data` arguments.

```javascript
return new Promise((resolve, reject) => {
        ec2.createTags(params, (err, data) => {
            if (err)
                reject(err);
            else
                resolve(data)
        })
    })
}
```

In this way, this will be our `createInstanceTag()` method:

```javascript
function createInstanceTag(instanceData, instanceTagName) {
    const params = {
        Resources: [instanceData.Instances[0].InstanceId],
        Tags: [
            {
                Key: 'Name',
                Value: instanceTagName
            }
        ]
    };

    return new Promise((resolve, reject) => {
        ec2.createTags(params, (err, data) => {
            if (err)
                reject(err);
            else
                resolve(data)
        })
    })
}
```

The functions are done! wow! And now?

Now we need to call all these functions and create and pass the needed arguments.

First, on the begin of our file (after all the needed aws settings and imports),
we will create some consts variables, the **`sgName`**
(the security group name), **`sgDescription`** (the security group description),
**`keyName`** (the key name for the KeyPair and the **`instanceTagName`** (the
name to be used on the Instance tag):

```javascript
const sgName = 'ec2_examples_security_group'; //The security group name
const sgDescription = 'ec2_examples Security Group description'; //the security group description
const keyName = 'ec2_examples_instance_key'; //The key for the KeyPair key
const instanceTagName = 'EC2 Examples'; //The name to be used on the Instance tag
```

And then, call the created functions passing the need arguments:

```javascript
createSecurityGroup(sgName, sgDescription)
    .then(() => {
        return createKeyPair(keyName);
    })
    .then(keyPairHelper.persistKeyPair)
    .then(() => {
        return createInstance(sgName, keyName, instanceTagName)
    })
    .then(createInstanceTag)
    .catch((err) => {
        console.error('Failed to create instance with:', err)
    });
```
So, in this way I will call first the `createSecurityGroup` function to create
the Security Group for our instance, then if everything works properly I will
call the `createKeyPair` function to create the needed KeyPair to log in the
instance, persisting in our local machine with our Helper function `persistKeyPair`,
to finish finally calling the `createInstance` function to create the instance
and then `createInstanceTag` to create the tag for this instance.

After create the calling of the functions, the script to create instances with the
NodeJs AWS-SDK is done.

Now, we can run this post example to create our instance.

# Running the script

To run the example, type the command `node create-ec2-instance.js` at the command line.

After a few seconds, it should print out the location of the key that was written
locally and the details of the instance that was just created.

![](/images/posts/aws/created-ec2-instance.png)

And shazam! The EC2 instance has been created only with JavaScript code!

The actual boot up of the instance can take a few minutes, but you already can
go to the [EC2 Console](https://console.aws.amazon.com/ec2) and look in the the
region specified on the begin of this example the instance that was created.

![](/images/posts/aws/ec2-instances.png)

We can look on the Security Group and Key Pairs options to see that they are
already created too.

The source of this example is available on my Github project [https://github.com/coderade/aws-ec2-examples/create-ec2-instance.js](https://github.com/coderade/aws-ec2-examples/blob/master/create-ec2-instance.js).

Take a look on it if you need a further information.

And that is all!

Thanks for your reading and please take a look at the other blog posts when you
have a time.

---
