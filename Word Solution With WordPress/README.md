# WORD SOLUTION WITH WORDPRESS
## Overview
This project is a documentation guide on how to prepare storage infrastructure on two linux servers and implement a basic wordpress solution using WordPress. WordPress is a  free and open source content management system written in PHP and paired with MySQL and MariaDB as its backend relational database management system (RDBMS)

### Project Tasks
1. Configure storage subsystem for web and Database servers based on Linux OS
2. Install WordPress and connect it to a remote MySQL database server

## Three-tier Architecture
Three-tier architecture is a client-server architecture pattern that divides applications into three logical and physical computing tiers, or layers. Each tier is responsible for a specific aspect of the application, ensuring separation of concerns and improving scalability, maintainability, and manageability. Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

**Three-tier Architecture** is a client-server software architecture pattern that comprise of 3 seperate layers.
![Three-tier application](https://github.com/user-attachments/assets/20d7c28f-2de9-4292-98ef-d97ca1345b99)

1. **Presentation tier**(Client tier): The presentation tier is the topmost layer of the application. It is responsible for interacting with the end users, presenting data, and capturing user input. This tier is often implemented using web browsers, mobile apps, or desktop applications. It provides a graphical interface for users to interact with the application. This includes rendering HTML, CSS, and JavaScript for web applications or native components for mobile and desktop applications. It captures user input, such as submission information, button clicks and other interactions, and receives data from the application tier and displays it to the users in a readable format 
2. **Business Layer**(Logic tier): The application tier, also known as the logic tier or middle tier, is responsible for the core functionality and business logic of the application. It acts as an intermediary between the presentation tier and the data tier, processing user inputs, making logical decisions, and coordinating data flow
3. **Data Access or Management tier**(Storage tier): The data tier is the bottom layer of the application architecture. It is responsible for storing, retrieving, and managing data. This tier ensures data persistence and provides mechanisms for data querying and manipulation.

   ### The 3-Tier Setup
1. A laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install WordPress)
3. An EC2 Linux server as a database (DB) server

   
