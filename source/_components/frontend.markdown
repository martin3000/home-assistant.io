---
layout: page
title: "Frontend"
description: "Offers a frontend to Home Assistant."
date: 2015-12-06 21:35
sidebar: true
comments: false
sharing: true
footer: true
logo: home-assistant.png
ha_category:
  - Other
ha_qa_scale: internal
ha_release: 0.7
---

This offers the official frontend to control Home Assistant.

```yaml
# Example configuration.yaml entry
frontend:
```

{% configuration %}
  javascript_version:
    description: "Version of the JavaScript to serve to clients. Options: `es5` - transpiled so old browsers understand it.  `latest` - not transpiled, so will work on recent browsers only. `auto` - select a version according to the browser user-agent. The value in the config can be overiden by putting `es5` or `latest` in the URL. For example `http://localhost:8123/states?es5` "
    required: false
    type: string
    default: auto
  themes:
    description: Allow to define different themes. See below for further details.
    required: false
    type: map
    keys:
      "[identifier]":
        description: Name to use in the frontend.
        required: true
        type: [list, map]
        keys:
          "[css-identifier]":
            description: The CSS identifier.
            required: true
            type: [list, string]
  extra_html_url:
    description: "List of additional [resources](/developers/frontend_creating_custom_ui/) to load in `latest` javascript mode."
    required: false
    type: list
  extra_module_url:
    description: "List of additional javascript modules to load."
    required: false
    type: list
  extra_js_url_es5:
    description: "List of additional javascript code to load in `es5` javascript mode."
    required: false
    type: list
  development_repo:
    description: Allow to point to a directory containing frontend files instead of taking them from a pre-built PyPI package. Useful for Frontend development.
    required: false
    type: string
{% endconfiguration %}


## {% linkable_title Defining Themes %}

Starting with version 0.49 you can define themes:

```yaml
# Example configuration.yaml entry
frontend:
  themes:
    happy:
      primary-color: pink
    sad:
      primary-color: blue
```

The example above defined two themes named `happy` and `sad`. For each theme you can set values for CSS variables. For a partial list of variables used by the main frontend see [ha-style.ts](https://github.com/home-assistant/home-assistant-polymer/blob/master/src/resources/ha-style.ts).

Check our [community forums](https://community.home-assistant.io/c/projects/themes) to find themes to use.

### {% linkable_title Theme automation %}

There are 2 themes-related services:

 - `frontend.reload_themes`: reloads theme configuration from your `configuration.yaml` file.
 - `frontend.set_theme(name)`: sets backend-preferred theme name.

Example in automation:

Set a theme at the startup of Home Assistant:

```yaml
automation:
  - alias: 'Set theme at startup'
    initial_state: 'on'
    trigger:
     - platform: homeassistant
       event: start
    action:
      service: frontend.set_theme
      data:
        name: happy
```

To enable "night mode":

```yaml
automation:
  - alias: 'Set dark theme for the night'
    initial_state: true
    trigger:
      - platform: time
        at: '21:00:00'
    action:
      - service: frontend.set_theme
        data:
          name: darkred
```

### {% linkable_title Manual Theme Selection %}

When themes are enabled in the `configuration.yaml` file, a new option will show up in the user profile menu (accessed by clicking your user account initials at the top of the sidebar). You can then choose any installed theme from the dropdown list and it will be applied immediately.

<p class='img'>
  <img src='/images/frontend/user-theme.png' />
  Set a theme
</p>

## {% linkable_title Loading extra HTML %}

Starting with version 0.53 you can specify extra HTML files to load.

Example:

```yaml
# Example configuration.yaml entry
frontend:
  extra_html_url:
    - https://example.com/file1.html
    - /file2.html
```

Those will be loaded via `<link rel='import' href='{{ extra_url }}' async>` on any page (states and panels).

### {% linkable_title Manual Language Selection %}

The browser language is automatically detected. To use a different language, go to the user profile menu (accessed by clicking your user account initials at the top of the sidebar) and select one. It will be applied immediately.

<p class='img'>
  <img src='/images/frontend/user-language.png' />
  Choose a Language
</p>
