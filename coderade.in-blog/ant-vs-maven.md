Ant VS Maven
This month I started to study the Maven tool too use in my projects. I Already used it in other projects,  but now I’m really sank my teeth into in the tool. When I started to learn this tool, I came across a your old relative that I already used it and it’s still used today: Ant.

So I found some differences between these two tools that I’ll show in this article.


A lot of people think that Ant and Maven are meant to compete against each other and really they can be used in conjunction with one another, but they’re really solving two different problems.

Ant
Ant was developed originally to be a replacement for a build tool called Make that wasn’t a sole design, but it.  Make wasn’t a cross platform tool.

Ant was built on top of Java and using XML, both tools that were meant to be used cross platform. So regardless of whether I’m on a Windows machine or a Mac or a Linux or a different environment, I can build things and have them be able to transfer from one environment to another.

Make was built around a Unix environment and was somewhat brittle, ran into problems with like whitespace and hidden characters, very powerful, was used for a lot of years, still used today, but very brittle in nature and not very cross platform.  Ant is built on top of Java and uses XML, but it’s very procedural. You have a hard time inheriting anything.

You have to go out of your way to use different pieces and have it stretched out to be able to use composition or things like that inside of your Ant scripts.

As I kind of mentioned, Ant isn’t really a build tool as much as it is a scripting tool. You have to do explicitly everything inside of Ant. You have to call out what your targets are. What I am going to, what goal I’m going to do next if I’m going to chain goals.

ant example
XHTML

<target name="clean" description="clean up">
    <!-- Delete the ${build} and ${dist} directory trees -->
    <delete dir="${build}" />
</target>
1
2
3
4
<target name="clean" description="clean up">
    <!-- Delete the ${build} and ${dist} directory trees -->
    <delete dir="${build}" />
</target>
Like the little code snippet above, what an Ant target is for just doing a clean. And it’s very clear that I can, I’m calling clean, I even got a description of what it is, I’ve got a comment thrown in there, and I’m going to clearly delete this directory, that’s our Build directory. You can see that the Build directory is a variable I’m passing in and it’s going to get evaluated at runtime.

But we have to define everything the way we want to do it. This actually can lead us to some problems. What if I like the word “clean,” you like the word “clear,” somebody else wants the word “clean up” for this target?

It can be left to any number of variations and  it’s not standard. So you have to go into every file and know that this is what I’m going to call for clean, this is what I’m going to call for clear or clean up.

Init is another one that I’ve seen people use from time to time as to what they would like to have their clean up procedure run as or delete or dist, delete distribution, that type of stuff.  So there is a lot of tribal knowledge that gets built into Ant, not specifically because I can rename variables or certain things, but because everybody does it different.

There is not a standard out there for what I’m going to call things. Each organization ends up having a large repository of these scripts that are unique to them. Nothing will carry over from one job to another or necessarily even one project to another. If I go to share this with another project, too, I have to go in and copy and paste all of these files to be able to reuse them.

So there is not a lot of reuse, there is no inheritance, there is no structure, nothing like that.

Maven  was not created to replace the Ant, but you can use to do many things it does.

Maven
Unlike Ant Maven is more than the scripting tool. It’s a full-fledged build tool, but you get a lot of implicit functionality built into Maven.

Maven clean is Maven clean. It doesn’t matter if I’m deleting the Target directory, generated sources, whatever I’m doing, whatever I’m going to clean up inside of application, it’s always going to be the clean goal.  You get a lot of consistency across projects that way.

With Maven  you’re also capable to achieve inheritance in projects by using parent POMs or inherited POM through composition, those types of things. You also get transitive dependencies (meaning that if I need to pull on a jar, it’s going to pull in all the jars that it needs to work within my application).

Nowadays you can do this using Ivy which is a not a newer tool per se, but an add-on to Ant. It wasn’t actually originally designed to work with Ant, but people have found that “Hey, I want some of this stuff with Ant. We’ve made too much of an investment with Ant to just walk away from it.” So you can use a tool like Ivy to manage transitive dependencies, but you’re really missing a lot of the other functionality that Maven has built into it.

Another key point worth mentioning with Maven is that it is built around versioning. What I mean by that is that it handles things, calling something at a snapshot inside of Maven actually has some context behind it. And we’ve got a whole section coming up on versioning. It’s  one of the key goals that Maven had in mind when it was designed.

Pros and cons of use Maven
Maven has many pros of your use, but also some cons that you should consider. So you need to think that some of the things that maybe aren’t so good about Maven or that it has a steeper learning curve.

Maven can be a black box, meaning that I don’t necessarily see where all this stuff is defined at. It’s got a steeper learning curve.
With Maven you’ve got the convention over configuration, meaning that if I follow Maven’s convention, it works really well for me.  But if I try to step outside their boundaries, things really start to crumble quite a bit.
There’s a lot better IDE integration with Maven, more so recently than Ant. In Ant, I can call goals from Maven or from my IDE, excuse me, but Maven has a much better, much richer integration with my IDE like Eclipse or IntelliJ.
There is a lot of less overhead through the use of repos now. Traditionally, Maven and Ant would require us to download all these files multiple times. Well, recently with Maven, we’ve started using local or corporate repositories so I don’t have to download the same file 20 times because I have 20 projects using it.
Maven, one last kind of pro-con on it and Maven has a different mindset, has a steeper learning curve and it’s a problem that I read in sites like the StackOverflow often is people trying to make Maven act like Ant. One is a scripting tool. One is a build tool.

Biggest complaint I see is “Well, I can do this with Ant, I can’t do this with Maven.” You need to quit making Maven try to act like Ant.

Talk about Ant a little bit. Ant is really clear that what you’re doing, you trace to through your files. You step through everything that you’re going to see to do these series of goals. It’s a lot quicker to learn, but it’s very copy and paste intensive. And if we’ve learned anything from software development, copying, pasting leads to other errors in down the road.

If I forget to change a variable or I get something inherited that I wasn’t expecting to get inherited or those types of problems. Definitely, larger project size in your source control using Ant over Maven, and I’m going to copy those files and I’m going to store them in CVS or some version or get, that’s just kind of the nature of what Ant does, if you know it did, it doesn’t have any history or notion of a repository that it pulls from. So, generally, a lot larger project size.

ANT build.xml example
Let’s look at a basic Ant build file:

antbuild.xml
XHTML

<project>
    <target name="clean">
        <delete dir="build">
    </target>

    <target name="compile">
        <mkdir dir="build/classes">
    </target>

    <target name="jar">
        <mkdir dir="build/jar">
        <jar destfile="build/jar/HelloWord.jar" basedir="build/classes">
            <manifest>
                <attribute name="Main-Class" value="data.HelloWors"
            </manifest>
        </jar>
    </target>

    <target name="run">
        <java jar="build/jar/HelloWord.jar" fork="true">
    </target>
</project>
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
<project>
    <target name="clean">
        <delete dir="build">
    </target>

    <target name="compile">
        <mkdir dir="build/classes">
    </target>

    <target name="jar">
        <mkdir dir="build/jar">
        <jar destfile="build/jar/HelloWord.jar" basedir="build/classes">
            <manifest>
                <attribute name="Main-Class" value="data.HelloWors"
            </manifest>
        </jar>
    </target>

    <target name="run">
        <java jar="build/jar/HelloWord.jar" fork="true">
    </target>
</project>
Just walk through it here, really simple, you can see that I’ve got this clean, compile, jar, and run command that I’m going to exercise here to do this.

So it’s pretty straightforward clean obviously deletes our directory. Compile makes our directory first to make sure that it did exists, and then  there’s a JavaC task there that you could see that compiles are code into this destination directory.

And then I’ve got a jar or a packaging command that goes out and says, “Hey, take that Build directory and go ahead and jar that up.” Now, one thing here without looking at this, this looks like a complete Ant build file.

There’s actually a lot of problems with this build file. I could call jar first because we haven’t set up the previous goals that this one depends on and my compile or clean may have not ran. And so I can actually get an erroneous jar that doesn’t contain the information I want it to to build this application.

So this is actually a very brittle build file and can enable me to skip steps while running my application or building it. So it’s not even a really good contract about what we’re trying to do with our application. But as you can see, it’s pretty clearly laid out what I’m trying to do. If I follow the steps I can achieve my build.

This is where I was talking about that tribal knowledge that kind of comes into play. And what I mean by that is that I know personally that I have to call Ant clean, Ant compile, Ant jar, and then Ant run for my application to work. Now, that’s a little bit of a false sense of security because I shouldn’t have to know anything if my build tool is doing what it’s supposed to be doing, at least at that level.

MAVEN pom.xml example
example.pom.xml

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/settings-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>com.coderade</groupId>
   <artifactId>HelloWord</artifactId>
   <version>0.0.1-SNAPSHOT</version>
</project>
1
2
3
4
5
6
7
8
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/settings-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>com.coderade</groupId>
   <artifactId>HelloWord</artifactId>
   <version>0.0.1-SNAPSHOT</version>
</project>
Now, the Maven POM file, I can do all of those goals that I just ran in that Ant file on this application. I can do clean, I can do compile, I can do jar, I can do run, and it will all work because it knows, looking at my code just from this Maven Pom file, this where I was talking about that it’s can be kind of a black box.

You don’t see all these stuff setup. If I follow their conventions, if I follow their naming conventions, their directory structures, that type of information, this application will run. So it’s a little bit nondescriptive at first until you understand the semantics of how Maven works.

So this is clearly a lot simpler, but it can also be more confusing at first.

Conclusion
In summary, Ant is very declarative. I have to lay everything out. Definitely has a shorter learning curve because I can see what I’m trying to do but I have to do everything. I have to write out every line of the script to do exactly how I wanted to execute what I want to do, how I wanted to do it.

Maven follows a convention over configuration model, basically meaning if I follow their naming conventions, things are just going to work a lot easier for me. Ant is easier to learn, but it’s really only meant to be a scripting tool. Maven is centered around managing your entire project’s life cycle.

So versions, how we structure our code, generate information about our application, and we’re going to dive into a lot more of what these feature are.
