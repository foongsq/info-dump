# Home

## Concept: Web Communication

**Why do we need a web server?**

A web server like NGINX communicates with the outside world.

Some browser requests (like html forms) don't have simple responses, so the browser needs to invoke an external script to process the html form input to get a response. In reality, the browser tells the web server that it wants to process a html form, then the web server forks a process (which inherits parent's environment variables, and executes the process to process a html form). 

When the browser generates html pages, NGINX passes it to gunicorn and flask, then flask will generate another page and send it to gunicorn, and sends it back to NGINX which sends it back to browser.

![Web Communication](./images/web_communication.png)

**Why do we need CGI and WSGI?**

Its hard to have a standardized script because everyone names environment variables differently, so cannot just fetch based on env variable name. Hence Common Gateway Interface (CGI) was born, it standardizes the environment variables names and their purpose.

![Common Gateway Interface Examples](./images/cgi_egs.png)

WSGI takes it a step further to standardize the way that the script is called. There will be a python function defined in the wsgi.py file, the first param is cgi env variables (in a python dictionary), then the second is another function that must be called once inside the python function. The purpose of WSGI is to standardize the way that external scripts are executed by web servers. Flask supports WSGI, WSGI is a communication protocol, between client web application and also server code.

**Why do we need an application server?**

We need gunicorn because loading external scripts takes a long time. The bottleneck of processing requests is the time it takes for web server to load python binary from hard drive and starting a new process (forking). 

**Solution:** pre-forking, starting the forking process when the web server is idle. Forking 3 times gives you 3 web server workers. Workers are in memory instances of python interpretor. pre-forking is web server functionality. NGINX cant prefork WSGI apps but can forward HTTP requests to another web server (gunicorn which understands HTTP requests) which can prefork your WSGI app.

## Concept: Web hosting

DNS server translates domain into an IP address, its a form of mapping.

When a hosting provider allocates space on a web server for a website to store its files, they are hosting a website. Web hosting makes the files that comprise a website (code, images, etc.) available for viewing online. Every website you’ve ever visited is hosted on a server. ([Source](https://www.namecheap.com/hosting/what-is-web-hosting-definition/))

The amount of space allocated on a server to a website depends on the type of hosting. The main types of hosting are shared, dedicated, VPS and reseller. They are differentiated by the kind of technology used for the server, the level of management provided and the additional services on offer.

Basically when you have the code for the website, you need to put that code onto a publicly available computer (ie. a web server) so that people can access it, then you need to link that publicly available computer to a domain name.

## Concept: Containerization in Web development

Containers isolate software from its environment and ensure that it works uniformly despite differences (eg. between development and production) ([Source](https://www.docker.com/resources/what-container)) Containers are a solution to the problem of how to get software to run reliably when moved from one computing environment to another. (eg. from physical machine to virtual machine in cloud)

## References
- [All you need to know about WSGI youtube video](https://www.youtube.com/watch?v=UklcIDgHtwQ)

