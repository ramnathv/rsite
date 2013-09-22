---
title: Books
framework: bootstrap3
layout: deck
mode: selfcontained
hitheme: solarized_light
url: {lib: ../libraries}
assets: {js: "/books/js/mustache.js", css: "/books/css/app.css"}
editlink: "books/issues.json"
---

<div class="container">
<div class="row">
    <div class="col-md-12">
        <p class="col-md-1">Filter by :</p>
        <ul id="filterBy" class='list-inline'>
            <li><a href="#" data-level="any" class="active" title="filter books by All">All</a></li>
            <li><a href="#" data-level="Beginner" title="filter books by Beginner">Beginner</a></li>
            <li><a href="#" data-level="Intermediate" title="filter books by Intermediate">Intermediate</a></li>
            <li><a href="#" data-level="Advanced" title="filter books by Advanced">Advanced</a></li>
        </ul>
    </div>
</div>

<!-- ////////// WRAPPER BOOKS -->
<div class="row" id="issueswrap"></div>
</div>


<script type="text/javascript">

function randOrd(){
return (Math.round(Math.random())-0.5); 
}

function filterBy() {
var // var's sort by
    active = 'active',
    btFilterBy = $('#filterBy').find('a'),
    booksection = $('.booksection');

// interaction filter by level
btFilterBy.on('click', function(e) {
    e.preventDefault();
    var level = $(this).data('level');
    if (level==='any') {
        // restore all books
        booksection.show();
    } else {
        booksection.not('.' + level).fadeOut('fast');
        booksection.filter('.' + level).fadeIn('fast');
    }

    btFilterBy.removeClass(active);
    $(this).addClass(active);
});
}

$(document).ready(function() {
// get the data and compile into the html template
$.getJSON('/books/issues.json?' + Math.random(), function(data) {
    var template = $('#booktpl').html(),
        issuesWrap = $('#issueswrap');

    data.books.sort(randOrd);
    issuesWrap.html(Mustache.to_html(template, data));

    filterBy();
});

});
</script>

--- .RAW 

<script id="booktpl" type="text/template">
  {{# books }}
  <div class='col-md-4 booksection {{level}}' style='margin-bottom: 20px;'>
  <div class="media">
  <a class="pull-left" href="{{ url }}" target="_blank">
    <img class="media-object" src="/books/{{cover}}" alt="..." width=150px height=200px>
  </a>
  <div class="media-body">
   <h4 class="media-heading">{{ title }}</h4>
   <h3><a href="{{authorUrl}}" target="_blank">{{author}}</a></h3>
   <p class="level">{{level}}</p>
   <p class="description">{{info}}</p>
  </div>
  </div>
  </div>
  {{/books}}
</script>
