Simple Sum Calculation using Google Native Client (NaCl)
===============================

Just a simple project to demonstrate how to use JavaScript to pass a message to C++ code to calculate the sum of two values and let C++ code then send back the sum to JavaScript and add it to the DOM


<a href="http://imgur.com/GhIXLgW"><img src="http://i.imgur.com/GhIXLgW.png" title="Hosted by imgur.com" /></a>


Key aspects

**in JavaScript** get the values and send them to C++ inside an object:

    Module = document.getElementById('module');    
    values = {
          "val1": Number(document.getElementById('val1').value),
          "val2": Number(document.getElementById('val2').value)
       	};
    msg = JSON.stringify(values);
    Module.postMessage(msg);
    

Then handle the message and send the response back to JavaScript

**in C++:**

In the header you have **picoJSON** to handle JSON and **sstream** to work with **isstringstream**:

    #include <sstream>
    #include "picojson.h"
    using namespace std;

then later in the code magic happens: 

    virtual void HandleMessage(const pp::Var& var_message) {
    
            picojson::value v;
    
            // pass the message that was sent from JavaScript as a string
            // var_message.AsString() will be in form "{\"val1\":4,\"val2\":4}");
            // and convert it to istringstream

            istringstream iss2((string)var_message.AsString());

            // parse iss2 and extract the values val1 and val2
            string err = picojson::parse(v, iss2);

            int val1 = (int)v.get("val1").get<double>();
            int val2 = (int)v.get("val2").get<double>();

            // finally send the message and you'll see the sum in the JavaScript
            PostMessage( val1 + val2 );
            
      }
