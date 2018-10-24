<template>
  <div id="app">

    <header id="fh5co-header" role="banner">
      <div class="container">
        <div class="header-inner">
          <h1><a href="index.html">PeopleJS</a></h1>
          <nav role="navigation">
            <ul>
              <li class="active"><router-link to="/">Home</router-link></li>
              <li><router-link to="/about">About</router-link></li>
              <li><router-link to="/about">Fake Link</router-link></li>
            </ul>
          </nav>
        </div>
      </div>
    </header>

    <router-view/>

    <footer id="fh5co-footer" role="contentinfo">
    
      <div class="container">
        <div class="col-md-6 col-md-push-1 col-sm-12 col-sm-push-0 col-xs-12 col-xs-push-0">
          <h3>Our Services</h3>
          <ul class="float">
            <li><a href="#">Web Design</a></li>
            <li><a href="#">Branding &amp; Identity</a></li>
            <li><a href="#">Free HTML5</a></li>
            <li><a href="#">HandCrafted Templates</a></li>
          </ul>
          <ul class="float">
            <li><a href="#">Free Bootstrap Template</a></li>
            <li><a href="#">Free HTML5 Template</a></li>
            <li><a href="#">Free HTML5 &amp; CSS3 Template</a></li>
            <li><a href="#">HandCrafted Templates</a></li>
          </ul>

        </div>
        
        
        <div class="col-md-12 fh5co-copyright text-center">
          <p>&copy; 2016 Free HTML5 template. All Rights Reserved. <span>Designed with <i class="icon-heart"></i> by <a href="http://freehtml5.co/" target="_blank">FreeHTML5.co</a> Demo Images by <a href="http://unsplash.com/" target="_blank">Unsplash</a></span></p>  
        </div>
        
      </div>
    </footer>

  </div>
</template>

<style>
</style>
