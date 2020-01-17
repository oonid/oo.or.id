---
title: "Search"
date: 2019-08-07T15:16:07+08:00
draft: false
---

<div class="search-results"></div>

<script src="https://unpkg.com/lunr/lunr.js"></script>

<!-- Generate a list of posts so we can display them -->

<script>
const query = new URLSearchParams(window.location.search);
const searchString = query.get('q');
const $postTitle = document.querySelector('.post-title');
const $searchResults = document.querySelector('.search-results');
$postTitle.innerHTML = 'search: '+searchString;
fetch('/search-index.json').then(function (res) {
  return res.json();
}).then(function (data) {
  const index = lunr.Index.load(data);
  const matches = index.search(searchString);
  const matchPosts = [];
  matches.forEach((m) => {
    matchPosts.push(m);
   console.log(m);
  });
  if (matchPosts.length > 0) {
    var results = '';
    matchPosts.forEach((p) => {
      results += '<ul class="posts-list"><li class="post-item">';
      results += '<a href="'+p.ref+'">'+p.ref+'</a>';  // title?
      results += '</li></ul>';
    });
    $searchResults.innerHTML = results;
  }
  else {
    $searchResults.innerHTML = '<div>No search results found</div>';
  }

});
</script>
