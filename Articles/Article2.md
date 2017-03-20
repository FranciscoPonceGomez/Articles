# Creating and deploying a Bot in Node.js on VS 2015

The Microsoft Bot Framework has been around since 2016. It offers the posibility of building your own conversational bot within a few clicks in a painsless registration process. You can use both a C# SDK and a Node.js SDK in order to be accesible to a broad range of developers. If you have ever built a web application in Node.js and published it in GitHub you probably are very familiar with Git commands.

But what about using your beloved Visual Studio IDE to build Node.js apps without punching a single Git command and deploying meaninglessly on Azure? Well, thanks to the Node Tools you can know do everything without leaving the VS 2015 IDE. If you follow these steps you will see how easy it is to create and deploy you Node.js app in VS 2015.

## Prerequisites

1. Download [Visual Studio 2015](https://www.visualstudio.com/post-download-vs/?sku=community&clcid=0x409&downloadrename=true) if you don't have it yet.
2. Enable [Node Tools](https://www.visualstudio.com/vs/node-js/) for Visual Studio.

## Create a Node.js app from one of the nunerous templates offered

1. Create the app in Visual Studio 2015.
2. Select one of the Node.js templates.

<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/newProject.png" /> Selecting a new project </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/NodeJSProject.png" /> Selecting a Node.js template </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/solutionStructure.PNG" /> Structure of the solution in VS 2015 </div> 


## Install all the necessary npm packages

If you come from the fascinating JavaScript world, you probably know what npm is. But for those C# developers out there that just want to dip their toes in the Node.js waters without leaving the confort of Visual Studio IDE I will explain it birefly.

### Overview

NPM is the default package manager for the JavaScript runtime environment Node.js. It consists of a command line client that interacts with a remote registry. It allows users to consume and distribute JavaScript modules that are available on the registry.

### Usage

NPM can manage packages that are local dependencies of a particular project, as well as globally-installed JavaScript tools. When used as a dependency manager for a local project, npm can install, in one command, all the dependencies of a project through the package.json file. In the package.json file, each dependency can specify a range of valid versions using the semantic versioning scheme, allowing developers to auto-update their packages while at the same time avoiding unwanted breaking changes. npm also provides version-bumping tools for developers to tag their packages with a particular version.

### How to install npm packages in VS 2015 with Node Tools

The tipical Node.js application calls all the modules it needs at the begining with a sintaxis like this:

```javascript
var restify = require('restify');
var builder = require('botbuilder');
```

In Visual studio you can add the necessary modules through Node Tools:

<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/npmPackages.png" /> Opening the npm Package Manager </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/npmPackages2.png" /> Selecting the necessary modules </div> 

This will be the equivalent of doing the folowing in npm command shell:

```npm
npm install restify --save
npm install botbuilder --save
```

## Deploy to Azure

Deployment into Azure from Visual Studio is pretty straightforward. On the solution explorer we just need to right click and select "Publish". From there we have to fill out the necessary information and login into our Azure account:


<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish.png" />  </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish2.PNG" />  </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish3.PNG" />  </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish4.PNG" />  </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish5.PNG" />  </div> 
<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish6.PNG" />  </div> 


Once everything is setup we can publish, and immediately after our Web App will whow up in our Azure Dashboard:

<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/publish7.PNG" />  </div> 

As you can see is very easy to create and deploy a project in Node.js without using more IDEs than VS 2015. Now you can create bots in both C# and Node.js without context changing and painfull CLI commands.
