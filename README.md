# AOS HTML UI

This project allows you to create a simple HTML UI for your AOS project. Effectively it
is a vue3 + vuetify3 wrapped into your AOS process.

In the process you define pages with html code, vuetify components and inputs which in
turn run AOS lua commands taking user to another page or updating the state.

This library converts each users state automatically to vue3 state upon update.
Commands to AOS send via buttons.

You can add your own components or remove currently used to reduce the size of the library.


## Installation

```bash

# in root folder
npm install

npm run build
npm run deploy -- <new_process_name>

```

This will create a new process in your AOS with the name <new_process_name>, bind generated
index.html (with vue3 + vuetify3) to Data, and load ui-library blueprint.

Now go to:

https://ar-io.dev/<new_process_id>


And you should see Hello World message.

Load ui-example.lua blueprint to see how to create UI for your process.


## Creating UI for your process

Example:

https://ar-io.dev/NOVn8BEuZS6uykWREfnHXvlt9oxayhewzec--kAE9vg


```lua

UI_APP = {

  PAGES = {
    {
      path = '/',
      html = [[
        <h1>UI Example</h1>
        <p>Enter your name:</p>
        <ui-input ui-id="name" ui-type="String" ui-required label="Your name" />
        <ui-input
          ui-id="fruit"
          ui-type="Select"
          ui-required
          label="Your fruit"
          :items="['Apple', 'Banana', 'Cherry', 'Mango']"
        />
        <ui-button ui-valid="name, fruit" ui-run="cmdLogin({ name = $name, fruit = $fruit })">Login<ui-button>
      ]]
    },
    {
      path = '/home',
      html = [[
        <h1 class="mb-2">Welcome, {~name~}!</h1>
        <p class="mb-2">What would you like to do with your {~fruit~}?</p>
        <ui-button ui-run="cmdLogout()">Logout</ui-button>
      ]]
    },
  },

  InitState = function (pid)
    return {
      name = "",
      fruit = ""
    }
  end,
}

function cmdLogin(args)
  UI.set({ name = args.name, fruit = args.fruit })
  return
    UI.page({ path = "/home" }) ..
    UI.state()
end


function cmdLogout(args)
  UI.set({ name = "" })
  return UI.page({ path = "/" })
end

```




