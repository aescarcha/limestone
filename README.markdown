## Persistent sphinx connector for NodeJs

This repository is a fork of the unmantained [limestone](https://github.com/kurokikaze/limestone) with some changes to fix bugs and specially to make the persistent connection work. It adds a method `persConnect` to force a persistent connection. ES6 Required

Here is an example of opening a persistent connection and running multiple queries in an interval:

    var limestone = require("./limestone").SphinxClient();

    llimestone.persConnect(9312).then( function(limestone) {
            console.log('Connected, sending queries');

            setInterval(() => {
                limestone.query({
                        'query' : 'dog', // query object with sphinx options
                        'maxmatches' : 70,
                        'limit': 70,
                        filters: [
                            {
                                attr: 'oneFilter',
                                values: [12]
                            },
                        ],
                        'indexes':'myindex, myotherindex'},
                    function(err, answer) {          // callback
                        console.log('Extended search yielded ' +
                            answer.match_count + " results\n" +
                            JSON.stringify(answer));
                       

                    });
            }, 1000);
        }).reject((e) => {
            console.log('Connection error: ' + err.message);
            process.exit();
        });


