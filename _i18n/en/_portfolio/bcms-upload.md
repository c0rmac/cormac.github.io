## Overview
In 2014 the National Building Control and Market Surveillance Office was established, under the implementation of the S.I.9 Building Control Amendment Regulations act, to provide oversight, support and direction for the development, standardisation and implementation of Building Control in the Irish state. Building Control related documents by all construction related professionals were therefore required to be submitted to the office for reviewal and public publication, which could be done through their Building Control Management System (a.k.a. BCMS) website.

It has been complained by many construction related firms that uploading, often between 50 to 200 documents per submission, was exceptionally slow. In some cases, it was reported that a 2mb document could take up to 5 minutes to submit. Documents have to be uploaded to the website individually by a user.

The solution to this problem was to create a proxy web application BCMS Upload Services which would allow for multiple document uploads at once. Documents are uploaded to the app in only a few seconds and would then be submitted to the official BCMS website in the background. Furthermore, the app is equipped with a basic queue management system to prevent too many submissions from being sent to the official BCMS website and to prevent performance issues from worsening.

[Click here](https://bcms-upload.ie) to use the app.

## Gallery
<div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
  <ol class="carousel-indicators">
    <li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="1"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="2"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="3"></li>
    <li data-target="#carouselExampleIndicators" data-slide-to="4"></li>
  </ol>
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img class="d-block w-100" src="/images/portfolio/bcms-upload/en.login.png" alt="Login">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src="/images/portfolio/bcms-upload/en.dashboard.png" alt="Dashboard">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src="/images/portfolio/bcms-upload/en.my_applications_supporting_docs.png" alt="My Applications/Supporting Documents">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src="/images/portfolio/bcms-upload/en.my_notices_supporting_docs.png" alt="My Notices/Supporting Documents">
    </div>
    <div class="carousel-item">
      <img class="d-block w-100" src="/images/portfolio/bcms-upload/en.my_notices_stat_docs.png" alt="My Notices/Statutory Documents">
    </div>
  </div>
  <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>

## Technicalities
The application is based on Kotlin Multiplatform with Common Module, Server Module and Internet Application Module. The server Module is divided into sub-modules based on a hexagonal architecture and there is a sub-module called `web-app` which holds the Spring Boot application. The Internet Application Module hosts a ReactJS application written in Kotlin-JS and based on the `trin-react` framework.

There is also a Wordpress plugin that lays out the main site.

For more information for those interested in the technical details, [see this Article](/posts/2023/1/bcms-upload-1/)