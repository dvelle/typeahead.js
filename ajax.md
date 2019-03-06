This is what worked for me:-

HTML Setup :

<!-- HTML Setup-->
<input type="text" id="my-typeahead" />
Javascript :

/* 
example remote-data-source
[
  {
    id:1,
    name:'Batman'
  },{
    id:2,
    name:'Superman'
  } 
]
*/

$('#my-typeahead').typeahead({
    hint: true,
    highlight: true,
    minLength: 1
}, {
    name: 'myDataset',
    source: function(str, sync, async) {
        // abstracting out the "REST call and processing" in a seperate function
        restCall(str, async);
    },
    templates: {
        suggestion: function(user) {
            return '<div>' + user.name + '</div>';
        },
        pending: '<div>Please wait...</div>'
    }
});

// The function that will make the REST call and return the data
function restCall(str, async) {
    return $.ajax('http://url-for-remote-data-source/query?name=' + str, {
        // headers and other setting you wish to set-up
        headers: {
            'Content-Type': 'application/json',
        },
        // response
        success: function(res) {
            //processing data on response with 'async'
            return async(res.data);
        }
    });
}
References:

Typeahead Docs - Datasets : https://github.com/twitter/typeahead.js/blob/master/doc/jquery_typeahead.md#datasets

Jquery **$.ajax()** : https://api.jquery.com/jquery.ajax/

Good Luck.
