# How we fought for High Availability

## Resources 
* Presentation Deck

## Installation
This presentation uses external markdown and speaker note, it is required that presentations run from a local web server. The following instructions will set up such a server.

1. Install [Node.js](http://nodejs.org/)

2. Install [Grunt](http://gruntjs.com/getting-started#installing-the-cli)

3. Clone the Presentation Repository
   ```sh
   $ git clone --recursive https://github.com/holser/openstack-summit-paris-2014.git
  ```

4. Navigate to openstack-summit-paris-2014 folder
   ```sh
   cd openstack-summit-paris-2014
   ```

5. Install dependencies
   ```sh
   $ npm install
   ```

6. Serve the presentation and monitor source files for changes
   ```sh
   $ grunt serve
   ```

7. Open <http://localhost:8000> to view your presentation

   You can change the port by using `grunt serve --port 8001`.

8. Press **s** to see speaker notes, **esc** to see the structure of presentation

9. Press **f** for presentation mode

## Authors

* [Vladimir Kuklin] (https://openstacksummitnovember2014paris.sched.org/speaker/vladimir_kuklin.1sq7etz9)
* [Sergii Golovatiuk] (https://openstacksummitnovember2014paris.sched.org/speaker/sgolovatiuk)
