---
title: "Assist - tips for getting started"
related:
  - docs: /docs/organizing/areas/
    title: Areas
  - docs: /docs/organizing/floors/
    title: Floors
  - docs: /voice_control/custom_sentences/
    title: Assist - custom sentences
---

There are a few things you should do to get the most out of the voice assistant experience.

Assist relies heavily on entity names, domains and areas. Below you will find tips for tweaking these things to ensure the best experience.

## Names and aliases

Not having good ways to address entities in common speech will greatly hinder your voice experience with Assist. You can expect to have a hard time asking Assist to *"turn on Tuya Light Controller 0E54B1 Light 1"*. You should therefore name your devices and entities logically, using a schema such as `<area> [<identifier_or_descriptor>] [<domain>]`.

For example, `light.living_room_lamp` might be the entity ID of `Living room lamp`. `Kitchen light`would be enough for the `light.kitchen` if you only have one light fixture in that room.

Note that this convention is only a recommendation, actual naming of your devices and entities might depend on your language or personal preference.

If you ever find yourself mentioning a certain device or entity in a certain way, make sure to add that as an alias, as it would probably be the most natural way to refer to the entity.

Names and aliases also apply to `area`, you need to address area names and aliases in the exact same manner as you would for entities.

### Language specificity

If you have set up Home Assistant entity names in English but plan to use Assist in another language, don't worry. You can add [aliases](aliases) to each entity, allowing them to be
named in any language.

English has pretty simple grammar rules, but there are languages where definite articles are pre- or suffixes to words, where nouns have genders or numbers. Language leaders are making efforts
to support most such declinations in each language, but they can't control the stuff that you name. So try to think whether a certain entity having an unarticled name would be called out in a
sentence requiring a definite article or viceversa. If so, add that version of the name as an alias as well.

### Area-less aliases for entities with an assigned area

It's good practice to add areas to entity canonical names, such as `Living room lamp`. However, since Assist can both infer the area and explicitly extract it from sentences, it's a very
good idea to add simplified aliases to all your exposed entities. In this case, having the `Lamp` alias set for the `Living room lamp` would allow you to `turn on the lamp in the living room` or simply `turn on the lamp`, when asking a satellite in the living room.

Don't worry if you also have a `Bedroom lamp`. You can alias that one `Lamp` as well, as it would get matched only when in conjunction with the area name (`Living room` or `Bedroom`).

## Domains and device classes

Assist leverages domains to define the proper verbs for the action being taken (e.g. turning on/off a `light` or a `fan`, opening/closing a `cover` with a `door` `device_class`,
opening/closing a `valve`, locking/unlocking a `lock` etc.).

It might not bother anyone to have a `switch.main_valve` in the UI instead of a `valve`, but you can't ask Assist to `open the main valve` if the main valve is a `switch`. If it was a
`valve.main_valve`, then the former sentence would have worked without a hitch.

To prevent this, you can use either the [Change device type of a switch](/integrations/switch_as_x/) integration or create virtual entities using [template entities](/integrations/template/)
or Generic X (e.g. [generic thermostat](/integrations/generic_thermostat/)).

The same thing applies to some device classes. For example, if you have a `binary_sensor.bedroom_window` with no `device_class` set, you can only ask whether the bedroom window is `on`, which
doesn't even make sense. To be able to ask if it's `open`, you need to set a proper `device_class` to that `binary_sensor`, i.e. `window`.

## Areas & floors

Assist tries to match area names, wither when provided or when inferred from context (e.g. the assigned `area` of a voice satellite). It uses that information intelligently to determine what
entity you are trying to manage, out of many with the same name/alias.

If you haven't properly assigned areas to your devices/entities so far, it would be a great time to do so in preparation for using Assist. Also, the same is valid for the more recently added
concept of floors.

## Exposing the bare minimum

It might be tempting to expose all entities to Assist, but doing so will come with a performance penalty. The more entity names and aliases the parser will have to go through, the more time it
will spend matching. And if you're using a LLM-based conversation agent, it will incur a higher cost per request, due to the larger context size.

As such, you should not expose anything by default, and then expose the entities you know you will control or query through voice. You're probably never going to ask if there's any update
available for the MQTT add-on, so there's no point in exposing the `update` entity to Assist.