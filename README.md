# Peanut Butter Sundays: map and assistant

I co-founded Peanut Butter Sundays, a youth-run nonprofit in LA. We started by handing PB&J
sandwiches to unhoused neighbors and it turned into basketball tournaments, skate meetups, and a
music festival that put housed and unhoused people in the same place. I run the tech side. This
repo is the two things I built for it: a map of where the need is, and the chat assistant on the
site. I pulled both out of the main website so the code stands on its own. The whole site is at
https://peanutbuttersundays.com.

Live demo: https://lllove514.github.io/pbs-tools/

## The map

An LA County map shaded by how many people are experiencing homelessness in each of the eight
Service Planning Areas, from the 2025 LAHSA count. Click a region and it opens that area's numbers
plus a slider that turns dollars into meals, because "16,955 people in Metro LA" lands harder when
you can watch what a couple hundred meals does against it. Plain JavaScript, Leaflet, one committed
GeoJSON of the county boundaries, no build step. The homelessness figures are the part I update by
hand when LAHSA publishes a new count.

## Jelly, the assistant

Jelly answers questions on the site about our programs, donating, and volunteering. The widget is
vanilla JS and keeps the conversation in memory, nothing is stored. The API key lives in a small
Cloudflare Worker so it never reaches the browser, and the Worker keeps Jelly on a short leash: it
only knows a fixed set of facts about PBS, declines off-topic questions, and points people to 988
or 211 if someone seems to be in crisis. It runs on Claude.

## Running it

The map needs nothing but a static server:

```
python3 -m http.server 8000
```

Then open http://localhost:8000/map/ for the map, or /chat/demo.html for the assistant. The
assistant's replies need the Worker running, see `worker/README.md` for the key setup.

## Honest notes

The map's meals math is deliberately blunt, two dollars a meal, meant to give a feel for scale and
not to be an accounting figure. Jelly tells you when it does not know something instead of guessing.
The homelessness data is only as current as the last count I loaded. Map method and sources are in
`map/README.md`; the Worker's guardrails are in `worker/README.md`.
