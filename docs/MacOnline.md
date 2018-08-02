# Mac Quick-start to Hello World with Developer Rig in Online Mode

Follow these steps to get Hello World running in Online Mode in the Rig.  This guide assumes you have either cloned the Rig from [GitHub](/twitchdev/developer-rig) or downloaded the Zip file from [Twitch](https://dev.twitch.tv).

Run these two scripts to install and configure the Developer Rig.  If the Developer Rig is already installed and configured, running these two scripts will have no effect.

1.  `./scripts/install`
2.  `./scripts/configure`

The `./scripts/install` script installs the following prerequisites.  If you already have these prerequisites, running this script will have no effect. You may install these yourself instead.  (It is possible to use [brew](https://brew.sh/) to install all of these.)

1.  [Node LTS](https://nodejs.org/en/download/).  **If you already have Node installed, it must be at least version 6.**
2.  [Yarn](https://yarnpkg.com/lang/en/docs/install).
3.  [Git](https://git-scm.com/download/mac).

The `./scripts/configure` script performs the following actions.  If the Developer Rig is already configured, running this script will have no effect. You may perform these actions yourself instead.  Note that the `./scripts/configure` script invokes the `./scripts/install` script.

1.  Install run-time dependencies.
    1.  `cd path/to/developer-rig`
    2.  `yarn install`  
        This takes about half a minute.
2.  Clone and configure the "Hello World" extension from GitHub.
    1.  `cd path/to/developer-rig`
    2.  `yarn extension-init -d ../my-extension`  
        This will clone the Hello World example from GitHub.  You may replace *../my-extension* with a different directory name here and in subsequent steps.
    3.  `cd ../my-extension`  
    4.  `npm install`  
3.  Add `127.0.0.1 localhost.rig.twitch.tv` to `/etc/hosts`.
    1.  `echo '127.0.0.1 localhost.rig.twitch.tv' | sudo tee -a /etc/hosts`
    2.  Provide your password if prompted.
4.  Create CA and Developer Rig and localhost SSL certificates.
    1.  `cd path/to/developer-rig`
    2.  `./scripts/make-cert`  

If you have not yet done so, create an extension manifest on your [Twitch Extensions Dashboard](https://dev.twitch.tv/dashboard/extensions) as described here.

1.  Visit https://dev.twitch.tv/dashboard/extensions.
2.  Click Create Extension.
3.  Provide all requested information about your new extension and click "Create Extension".  You may select as many types as are appropriate for your extension.  If you don't know, select "Panel".  You may change this later.
4.  Keep this browser session open since you'll need some of the data on it to create a configuration file later.

Note that, in the above steps, you didn't actually create an extension.  What you did was create Twitch's awareness of your extension (i.e., an internal manifest for your extension).  Your extension actually consists of all of the assets (e.g. HTML, JavaScript, images, etc.) that constitute the implementation of your extension.

If you already have an implementation, reference its front-end directory in step 4.iii below and its back-end in step 4.iii below.  If you don't have a back-end, you may skip step 4 below.

Follow these steps to launch the Developer Rig in Online Mode.

1.  Create a panel extension manifest file.
    1.  `cd path/to/developer-rig`
    2.  `yarn create-manifest -t panel -o ../panel.json`
2.  Host the front-end extension asset directory.
    1.  `yarn host -d ../my-extension/public -p 8080 -l`  
        *../my-extension/public* is the public folder of the hello-world example extension created in the previous step.
        **NOTE:**  this terminal window command will not exit.
3.  Visit https://localhost.rig.twitch.tv:8080/panel.html.  You will see `Hello, World!` in the browser window.
4.  Open a new terminal window and run these commands to run your Hello World back-end.
    1.  `cd path/to/my-extension`  
    3.  `node services/backend -l ../panel.json`  
        *../panel.json* is the path to the file created in step 3.iv above.
        **NOTE:**  this terminal window command will not exit.
5.  Visit https://localhost:8081.  You will see some JSON describing a 404 response in the browser window.
6.  Open a new terminal window and run these commands to run the Developer Rig.
    1.  `cd path/to/developer-rig`
    2.  `yarn start -l ../panel.json`  
        *../panel.json* is the path to the file created in step 1.ii above
        **NOTE:**  this terminal window command will not exit.
7.  Verify the Developer Rig is working.
    1.  Sign in with your Twitch credentials.
    2.  Click the `+` button. The "Add a new view" panel will appear.
    3.  Select the `Broadcaster` viewer type and click `Save`. The Broadcaster view will appear.
    4.  Click `Yes, I would`. Verify the color changes and there is output to match that request in the second terminal window.
    5.  Click the `+` button again. The "Add a new view" panel will appear again.
    6.  Select the `Logged-Out Viewer` viewer type and click `Save`. The Logged-Out Viewer view will appear.
    7.  Click `Yes, I would` in the new view. Verify the color changes in both views and there is output to match that request in the second terminal window.

When you are done using the Developer Rig, you may either close all opened terminal windows or press `Ctrl-C` in all of them.
