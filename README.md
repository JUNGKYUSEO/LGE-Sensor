[![](https://img.shields.io/github/release/ollo69/ha-smartthinq-washer/all.svg?style=for-the-badge)](https://github.com/ollo69/ha-smartthinq-sensors/releases)
[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)
[![](https://img.shields.io/github/license/ollo69/ha-smartthinq-washer?style=for-the-badge)](LICENSE)
[![](https://img.shields.io/badge/MAINTAINER-%40ollo69-red?style=for-the-badge)](https://github.com/ollo69)
[![](https://img.shields.io/badge/COMMUNITY-FORUM-success?style=for-the-badge)](https://community.home-assistant.io)

# LG ThinQ Devices integration for HomeAssistant
A Homeassistant custom component to monitor and control LG Air Conditioner, Washer, Dryer, DishWasher, Refrigerator, Styler and Range 
using ThinQ API based on [WideQ project][wideq].<br/>

**Important Version note:** 
1. From version 0.3.x component name changed from `smartthinq_washer` to `smartthinq_sensors`
If you are upgrading to version 0.3.x from previous version, you must **remove component configuration and uninstall component**
from Home Assistant before upgrade.<br/>

**Important**: The component will **not work if you have logged into the ThinQ application and registered your devices using a social network account** (Google, Facebook or Amazon). In order to use the component you need to create a new independent LG account and make sure you log into the ThinQ app and associate your devices with it.
If during configuration you receive the message "No SmartThinQ devices found", probably your devices are still associated with the social network account. To solve the problem perform the following step:
- remove your devices from the ThinQ app
- logout from the app and login again with the independent LG account
- reconnect the devices in the app

**Important 2**: If you receive an "Invalid Credential" error during component configuration/startup, check in the LG mobile app if is requested to accept new Term Of Service.

**Note**: some device status may not be correctly detected, this depends on the model. I'm working to map all possible status developing the component in a way to allow to configure model option in the simplest possible way and provide update using Pull Requests. I will provide a guide on how update this information.<br/>

## Installation
You can install this component in two ways: via HACS or manually.

### Option A: Installing via HACS
If you have HACS, just go in the Integration Tab and search the "SmartThinQ LGE Sensors" component to install it.

### Option B: Manually installation (custom_component)
Prerequisite: SSH into your server. [Home Assistant Add-on: SSH server](https://github.com/home-assistant/hassio-addons/tree/master/ssh)
1. Clone the git master branch.  
`git clone https://github.com/ollo69/ha-smartthinq-sensors.git`
2. If missing, create a `custom_components` directory where your configuration.yaml file resides. This is usually in the config directory of homeassistant.  
`mkdir ~/.homeassistant/config/custom_components`
3. Copy the smartthinq_sensors directory within the `custom_components` directory of your homeassistant installation from step 2.  
`cp -R ha-smartthinq-sensors/custom_components/smartthinq_sensors/ ~/.homeassistant/config/custom_components`
4. (Optional) Delete the git repo.  
`rm -R ha-smartthinq-sensors/`

    After a correct installation, your configuration directory should look like the following.
    ```
        └── ...
        └── configuration.yaml
        └── secrects.yaml
        └── custom_components
            └── smartthinq_sensors
                └── __init__.py
                └── config_flow.py
                └── const.py
                └── ...
    ```

5. Reboot HomeAssistant

## Component configuration    
Once the component has been installed, you need to configure it in order to make it work.
There are two ways of doing so:
- Using the web interface (Lovelace) [**recommended**]
- Manually editing the configuration.yaml file

### Option A: Configuration using the web UI [recommended]
Simply add a new "integration" and look for "SmartThinQ LGE Sensors" among the proposed ones and
**follow setup worflow**.<br/>

**Important**: use your country and language code: SmartThinQ accounts are associated with a specific locale, 
so be sure to use the country and language you originally created your account with. 
Reference for valid code:<br/>

- Country code: [ISO 3166-1 alpha-2 code][ISO-3166-1-alpha-2]<br/>
- Language code: [ISO 639-1 code][ISO-639-1]<br/>

### Option B: Configuration via editing configuration.yaml [deprecated - not possible from v0.5.4]
Follow these steps only if the previous configuration method did not work for you.<br/>
**Note**: with this configuration the integration will use APIv1 that cannot connect to new LG devices.
This configuration option is deprecated and is not possible starting from version 0.5.4<br/>

1. Enable the component by editing the configuration.yaml file (within the config directory as well).
Edit it by adding the following lines:
    ```
    smartthinq_sensors:
      token: my_smartthinq_token
      region: my_smartthinq_region #(ISO 3166-1 alpha-2, e.g. US)
      language: my_smartthinq_language #(ISO-639-1, e.g. en)
    ```

2. Reboot HomeAssistant

## Docs
In this example, "My [insert thing]" will just be the placeholder
#### Entities
| Entity ID | Entity Name | Description |
| :-- | :-: | :-- |
| sensor.my_washer | My Washer | Washer, turns On when on, turns Off when off |
| binary_sensor.my_washer_wash_completed | My Washer Wash Completed | Turns On when washer completed wash. You can use it in automations by triggering them when it goes from Off to On. |
| binary_sensor.my_washer_error_state | My Washer Error State | Off/OK means that it's fine. On/Error means there's an error. |
| sensor.my_dryer | My Dryer | Dryer, turns On when on, turns Off when off |
| binary_sensor.my_dryer_dry_completed | My Dryer Dry Completed | Turns On when dryer completed wash. You can use it in automations by triggering them when it goes from Off to On. |
| binary_sensor.my_dryer_error_state | My Dryer Error State | Off/OK means that it's fine. On/Error means there's an error. |

#### Attributes `sensor.my_washer`
Note: When something doesn't apply and/or is off, it may have a `-` as its value. Also, these are for @KTibow's washer, values may differ for yours. Feel free to open an issue/PR.
<details><summary>Hidden, click to expand</summary>

| Attribute ID | Description |
| :-- | :-- |
| model | Model ID of washer |
| mac_address | Mac address of washer |
| run_completed | Turns On when washer completed wash, just like binary_sensor.my_washer_wash_completed. |
| error_state | Off/OK means that it's fine. On/Error means there's an error, just like binary_sensor.my_washer_error_state. |
| error_message | When there is an error, this is what it is. (Format unknown) |
| run_state | Current state of washer in words |
| pre_state | Previous state of washer in words |
| current_course | Current washing cycle in words |
| spin_speed | Current cycle's spin mode in words |
| water_temp | Current cycle's water temperature in words |
| dry_level | Unknown attribute, might be used in combined washers and dryers for the current cycle's dry level |
| tubclean_count | How many cycles have been ran without running the Tub Clean cycle |
| remain_time | How much more time is remaining, H:MM |
| initial_time | The orginal amount of time, H:MM |
| reserve_time | When in Delay Start mode, the delay amount, H:MM |
| door_lock | Whether washer door is locked, on/off |
| child_lock | Whether child lock is on, on/off |
| remote_start | Whether remote start is enabled, on/off |
| steam | Whether steam is enabled on supported washers, on/off |
| pre_wash | Whether using prewash cycle, on/off |
| turbo_wash | Whether Turbowash is enabled, on/off |

</details>

#### Attributes `sensor.my_dryer`
Note: When something doesn't apply and/or is off, it may have a `-` as its value. Also, these are for @KTibow's dryer, values may differ for yours. Feel free to open an issue/PR.
<details><summary>Hidden, click to expand</summary>

| Attribute ID | Description |
| :-- | :-- |
| model | Model ID of dryer |
| mac_address | Mac address of dryer |
| run_completed | Turns On when dryer completed dry, just like binary_sensor.my_dryer_dry_completed. |
| error_state | Off/OK means that it's fine. On/Error means there's an error, just like binary_sensor.my_dryer_error_state. |
| error_message | When there is an error, this is what it is. (Format unknown) |
| run_state | Current state of dryer in words |
| pre_state | Previous state of dryer in words |
| current_course | Current drying cycle in words |
| temp_control | Current option for dryer temperature in words |
| dry_level | Current level for how much to dry |
| remain_time | How much more time is remaining, H:MM |
| initial_time | The orginal amount of time, H:MM |
| reserve_time | Unknown attribute, it could be this behaves the same as the washer's `reserve_time`, H:MM |
| child_lock | Child lock, on/off |

</details>

#### Examples (washer/dryer)
- Get a notification when the clothes are done drying (or when the clothes are done washing, automation)
```yaml
- id: 'dry_clothes_notification'
  alias: Dry clothes notification
  description: Alert when dryer finishes
  trigger:
  - entity_id: binary_sensor.my_dryer_dry_completed
    platform: state
    from: 'off'
    to: 'on'
  condition: []
  action:
  - data:
      title: 'The clothes are dry!'
      message: 'Get them while they're hot!'
    service: notify.notify
```
Substitute "dry" and "dryer" for "wet" and "washer" if you want to use with a washer.
- Custom card for dryer and washer (![Screenshot of laundry card](/washerpics/cardpic.png))
<details><summary>Hidden, click to expand</summary>

Put this file in `/config/www/laundry.js`, and add a custom resource in **HA UI** > **Sidebar** > **Config** > **Dashboards** > **Resources** > **Plus** > **Add `/local/laundry.js`**.
```js
// card-tools for more-info. MIT license (This isn't a substantial portion)
function lovelace_view() {
  var root = document.querySelector("hc-main");
  if(root) {
    root = root && root.shadowRoot;
    root = root && root.querySelector("hc-lovelace");
    root = root && root.shadowRoot;
    root = root && root.querySelector("hui-view") || root.querySelector("hui-panel-view");
    return root;
  }
  root = document.querySelector("home-assistant");
  root = root && root.shadowRoot;
  root = root && root.querySelector("home-assistant-main");
  root = root && root.shadowRoot;
  root = root && root.querySelector("app-drawer-layout partial-panel-resolver");
  root = root && root.shadowRoot || root;
  root = root && root.querySelector("ha-panel-lovelace");
  root = root && root.shadowRoot;
  root = root && root.querySelector("hui-root");
  root = root && root.shadowRoot;
  root = root && root.querySelector("ha-app-layout #view");
  root = root && root.firstElementChild;
  return root;
}
function fireEvent(ev, detail, entity=null) {
  ev = new Event(ev, {
    bubbles: true,
    cancelable: false,
    composed: true,
  });
  ev.detail = detail || {};
  if(entity) {
    entity.dispatchEvent(ev);
  } else {
    var root = lovelace_view();
    if (root) root.dispatchEvent(ev);
  }
}
function moreInfo(entity, large=false) {
  const root = document.querySelector("hc-main") || document.querySelector("home-assistant");
  fireEvent("hass-more-info", {entityId: entity}, root);
  const el = root._moreInfoEl;
  el.large = large;
  return el;
}
class LgLaundryCard extends HTMLElement {
  set hass(hass) {
    const entityId = this.config.entity;
    const state = hass.states[entityId];
    if (!state) {
        throw new Error('Entity not found. Maybe check to make sure it exists.');
    }
    const stateStr = state ? state.state : 'unavailable';
    const friendlyName = state.attributes.friendly_name;
    const friendlyNameStr = friendlyName ? " " + friendlyName : "";
    const courseName = state.attributes.current_course;
    const courseNameStr = courseName ? " " + courseName : "an unknown cycle";
    const stageName = state.attributes.run_state;
    const stageNameStr = stageName ? " " + stageName : "unknown";
    const iconName = state.attributes.icon;
    const iconNameStr = iconName ? iconName : "";
    const remainTime = state.attributes.remain_time;
    const remainTimeStr = remainTime ? remainTime : "unknown";
    const totalTime = state.attributes.initial_time;
    const totalTimeStr = totalTime ? totalTime : "unknown";
    var worked;
    var percentDone;
    try {
        const minRemain = (parseInt(remainTimeStr.split(":")[0]) * 60) + parseInt(remainTimeStr.split(":")[1]);
        const minTotal = (parseInt(totalTimeStr.split(":")[0]) * 60) + parseInt(totalTimeStr.split(":")[1]);
        percentDone = String(Math.round((minTotal - minRemain) / minTotal * 100)) + "%";
        worked = !isNaN(Math.round((minTotal - minRemain) / minTotal * 100));
    } catch(err) {
        console.log(err);
        worked = false;
    }
    if (!this.content) {
      this.contenta = document.createElement('a');
      this.contenta.href = "#";
      this.contenta.style.textDecoration = "unset";
      this.contenta.style.color = "unset";
      function laundryinfo() {
          window.history.pushState({}, "", window.location.href.split("#")[0]);
          moreInfo(this.entityId);
      }
      this.contenta.onclick = laundryinfo.bind({entityId: entityId});
      this.content = document.createElement('div');
      this.content.style.padding = '0 16px 16px';
      this.content.style.display = 'flex';
      const card = document.createElement('ha-card');
      card.header = friendlyNameStr;
      this.contenta.appendChild(card);
      card.appendChild(this.content);
      this.appendChild(this.contenta);
    }
    var conthtml = '';
    conthtml = `
      <ha-icon icon="${iconNameStr}" style="transform: scale(3,3); color: ${stateStr == 'on' ? "var(--sidebar-selected-icon-color)" : "var(--secondary-text-color)"}; display: block; padding: 8px 9px 12px 5px; margin: 15px;"></ha-icon>
      <div>${friendlyNameStr} is currently ${stateStr}.
    `;
    if (stateStr == 'on') {
        conthtml += `
          <br/>The ${courseNameStr} progress is ${stageNameStr}.
          <br/>There's ${remainTimeStr} remaining out of ${totalTimeStr} total.
        `;
        if (worked) {
          conthtml += `
            <br/>
            <div style="width: 100%; height: 30px; background-color: #8f89; display: inline-block;">
              <div style="max-width: 0; min-width: 0; max-width: ${percentDone} !important; min-width: ${percentDone} !important; height: 30px; background-color: #09d9; display: inline-block;">
                <b style="line-height: 30px; margin: 0 10px; display: block;">${percentDone}</b>
              </div>
            </div>
            </div>
          `;
          conthtml = conthtml.replace("8px 9px 12px 5px", "16px 9px 12px 5px");
        } else {
            conthtml += "</div>";
        }
    } else {
        conthtml += "</div>";
    }
    this.content.innerHTML = conthtml;
  }
  setConfig(config) {
    if (!config.entity) {
      throw new Error('You need to define a laundry entity');
    }
    this.config = config;
  }
  getCardSize() {
    return 3;
  }
  static getStubConfig() {
    return { entity: "sensor.my_washing_machine" };
  }
}

customElements.define('lg-laundry-card', LgLaundryCard);
window.customCards.push({type: "lg-laundry-card", name: "LG laundry card", description: "Card for LG laundry machines."});
```

Lovelace:
```yaml
type: 'custom:lg-laundry-card'
entity: 'sensor.the_dryer_dryer' # Washers work too!
```

</details>

- Alternative: Washer picture status card (LG전자 / CC BY (https://creativecommons.org/licenses/by/2.0) for image. Find the images [here](/washerpics/))
<details><summary>Hidden, click to expand</summary>
    
configuration.yaml:
```yaml
sensor:
  - platform: template
    sensors:
      washer_cycle_state:
        value_template: '{{state_attr(''sensor.my_washer'', ''remain_time'')}}'
        friendly_name: Washer Cycle State
        icon_template: 'mdi:washing-machine'
```
lovelace:
```yaml
cards:
  - type: conditional
    conditions:
      - entity: sensor.my_washer
        state: "on"
    card:
      aspect_ratio: '1'
      entity: sensor.washer_cycle_state
      image: /local/washerrunning.gif
      type: picture-entity
  - type: conditional
    conditions:
      - entity: sensor.my_washer
        not_state: "on"
    card:
      aspect_ratio: '1'
      entity: sensor.my_washer
      image: /local/washer.jpg
      type: picture-entity
type: vertical-stack
```

</details>

## Be nice!
If you like the component, why don't you support me by buying me a coffee?
It would certainly motivate me to further improve this work.

[![Buy me a coffee!](https://www.buymeacoffee.com/assets/img/custom_images/black_img.png)](https://www.buymeacoffee.com/ollo69)


Credits
-------

This component is developed by [Ollo69][ollo69] based on [WideQ API][wideq].<br/>
Original WideQ API was developed by [Adrian Sampson][adrian] under license [MIT][].

[ollo69]: https://github.com/ollo69
[wideq]: https://github.com/sampsyo/wideq
[adrian]: https://github.com/sampsyo
[mit]: https://opensource.org/licenses/MIT
[ISO-3166-1-alpha-2]: https://en.wikipedia.org/wiki/ISO_3166-2
[ISO-639-1]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
