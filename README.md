# Zabbix Matrix webhook

[![CI](https://github.com/jooola/zabbix-matrix-webhook/actions/workflows/ci.yml/badge.svg)](https://github.com/jooola/zabbix-matrix-webhook/actions/workflows/ci.yml)

This guide describes how to integrate your Zabbix installation with Matrix using the Zabbix webhook feature. This guide will provide instructions on setting up a media type.

## Matrix configuration

The configuration of the webhook use [Element](https://element.io/) to interact with the Matrix server, but receiving notification from Zabbix work with any [Matrix client](https://matrix.org/clients/).

### Create a Matrix account

Visit [app.element.io](https://app.element.io/) and create a new account for your bot.

### Retrieve the bot's access token

Open a **private/incognito** window in you browser, and sign in [app.element.io](https://app.element.io) with the bot account:

![](docs/matrix_access_token1.png)

Open the bot account settings pages:

![](docs/matrix_access_token2.png)

In the settings page, open the `Help & About` tab, scroll to the bottom `Advanced` section and click on `<click to reveal>` to retrieve the access token:

![](docs/matrix_access_token3.png)

Finally, **do not log out** (logging out invalidate the access token), instead, just close the **private/incognito** window.

### Set up a Matrix room

Invite the bot in the room you want the alerts to be sent.

In the room settings page, open the `Advanced` tab and retrieve the `Internal room ID`:

![](docs/matrix_room_id1.png)

## Zabbix webhook configuration

In order to configure the webhook, you will have to know:

- [How to import media types](https://www.zabbix.com/documentation/current/manual/xml_export_import/media)
- [How to configure webhooks](https://www.zabbix.com/documentation/current/manual/config/notifications/media/webhook)
- [To to send messages in Zabbix](https://www.zabbix.com/documentation/current/manual/config/notifications/action/operation/message)

First, [import](https://www.zabbix.com/documentation/current/manual/xml_export_import/media#importing) the Matrix webhook into your Zabbix installation:

- Use the [`latest/media_matrix.yml`](latest/media_matrix.yml) file for Zabbix 6.0 LTS and 6.2.
- Use the [`5.0/media_matrix.json`](5.0/media_matrix.json) file for Zabbix 5.0 LTS.

Then, in the media type configuration, fill the `<matrix_url>` value with the Matrix server [URL](https://en.wikipedia.org/wiki/URL) and `<matrix_token>` with the previously retrieved access token.

> To add a link to the event in your alert messages, you need to fill the `event_url` parameter with the url below. Make sure that your replace `<zabbix_url>` with the url of your Zabbix server.
>
> ```
> https://<zabbix_url>/tr_events.php?triggerid={TRIGGER.ID}&eventid={EVENT.ID}
> ```

![](docs/zabbix_media_type1.png)

Finally, create or configure the `Matrix` media type for your user, and paste the `Internal room ID` in the `Send to` field:

![](docs/zabbix_room_id1.png)
