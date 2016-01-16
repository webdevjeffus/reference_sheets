<h1>DBC Phase 2 Reference Sheet:<br/>
AJAX Calls</h1>

<h4>Prepared by Jeff George for future DBC students<br/>
Based on the Phase 2 Helper Sheet compiled by Ryan Lesson</h4>

**AJAX** allows a webpage to add, remove, and refresh DOM elements without refreshing the entire page, allowing for a smoother user experience. AJAX requests typically return either HTML strings or JSON objects.

### AJAX Examples
These examples are taken from my Hacker News Clone challenge.
```JS
$(document).ready(function() {

  // Append new comment to bottom of comments list
  // (Adding new HTML element to DOM)
  $("#post-comment-form").on("submit", function(event) {
    event.preventDefault();
    console.log(event);
    $.ajax({
      method: 'POST',
      url: $(event.target).attr("action"),
      data: $(event.target).serialize()
    }).done( function(response) {
      console.log(response);
      $("#show-comment-div").append(response);
      $('#post-comment-form').trigger("reset");
    }).fail( function(error) {
      console.log("Error: " + error);
    });
  });

  // Register vote (create vote object) and update count on screen
  // (Receiving JSON response and inserting value into DOM)
  $("#vote-btn-form").on("submit", function(event) {
    event.preventDefault();
    original_event = event;
    $.ajax({
      method: "POST",
      url: $(event.target).attr("action"),
      dataType: "json",
      data: $(event.target).serialize()
    }).done( function(response) {
      console.log(response);
      $("#votes-for-" + response.article_id).html(response.vote_count);
    }).fail( function(errors) {
      console.log(errors);
    });
  });

  // Insert edit comment button below comment in comment list
  $("#show-comment-div").on("click", ".edit-comment-btn", function(event) {
    event.preventDefault();
    original_event = event;
    $.ajax({
      method: "GET",
      url: $(event.target).attr("href"),
      // data not needed with GET requests
    }).done( function(response) {
      console.log(response);
      wrapper = $(original_event.target).closest(".comment-wrapper")
      wrapper.empty()
      wrapper.append(response);
    }).fail( function(errors) {
      console.log(errors);
    })
  })

  // PUT edited comment, remove form, and restore attribution line
  $("#show-comment-div").on("submit", ".put-comment", function() {
    event.preventDefault();
    original_event = event;
    $.ajax({
      method: "PUT",
      url: $(event.target).attr("action"),
      data: $(event.target).serialize()
    }).done( function(response) {
      console.log(response);
      $("#show-comment-div").append(response);
      original_event.target.remove();
    }).fail( function(error) {
      console.log(error);
    });
  })
});
```
