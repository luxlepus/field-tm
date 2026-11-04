# Mapping With GNSS Interference

Satellite positioning (GPS, Galileo and others) is increasingly jammed or
spoofed, especially near conflict zones, borders and sensitive sites. This
page helps mappers notice interference, keep their data correct and stay
safe, and helps project managers find affected submissions.

See also the background discussion in
[issue #3065](https://github.com/hotosm/field-tm/issues/3065).

## What Is Happening

**Jamming:** a strong radio signal drowns out the satellites. The phone loses
its position or reports it very inaccurately.

- No position, or it takes minutes to get one.
- The accuracy circle grows to hundreds of metres.
- Satellites are listed but not used for the fix.

**Spoofing:** fake satellite signals tell the phone it is somewhere else. The
position often looks precise and stable, which makes spoofing harder to
notice than jamming.

- The position jumps far away, sometimes to another town or an airport.
- The dot stays still while you walk, or moves in the wrong direction.
- Several phones in the team show the same wrong spot.

!!! warning

    Ordinary phones cannot reliably prove that a position is genuine. Treat
    any position that does not match what you see around you as wrong, even
    when the app shows good accuracy.

## For Mappers

### The 10-Second Check

Before you start, and every time something feels off, compare the blue dot
with the map. Find something you can see right now on the satellite imagery:
a road junction, a building corner, a field edge.

**If the dot is not where you are standing, do not use GPS for this
session.** Select or place features by what you see on the imagery instead.

### ODK Collect

- For existing features, pick the feature on the map by matching it with the
  imagery, not by its distance to the blue dot.
- For new features, place the point manually on the map. Do not use
  "current location".
- Make sure offline basemaps are on the device before you go, so imagery is
  available without a signal.
- If your form has a notes field, write "GPS unreliable, placed manually".

### QField

- Turn off "lock to position" and digitise by tapping on the imagery.
- In **Settings > Positioning**, enable the accuracy requirement so QField
  refuses to collect while accuracy is bad. This helps against jamming, not
  against spoofing.
- Position averaging reduces noise. It does not fix a spoofed position.
- An external multi-band receiver is more robust, but not immune either.

### Optional Tools

- **GPSTest** (open source, Android) shows which satellites the phone sees and
  their signal strength.
- **GnssLogger** by Google has a Spoof/Jam view. According to the
  [Android documentation](https://developer.android.com/develop/sensors-and-location/sensors/gnss-spoof-jam),
  signal strength (C/N0) and automatic gain control (AGC) dropping together
  point to jamming, while C/N0 rising as AGC drops points to spoofing.
- Neither tool can prove that a position is genuine. Use them to confirm a
  suspicion, not to dismiss one.

## Safety and Data Protection

!!! danger

    GNSS interference often happens near military activity, borders and
    sensitive sites. Mapper safety comes before any data. Follow your
    organisation's security rules and stop mapping if the situation feels
    unsafe.

- Never use mock-location apps or your own transmitters to "fix" the
  position.
- Each submission records the mapper's own location. In Field-TM ODK forms
  this is the `warmup` field (a `start-geopoint`), together with
  `deviceid` and `username`. In conflict areas this is sensitive personal
  data.
- Limit who can download raw exports, and remove location, device and
  username columns before sharing data.
- Do not publish where and when individual mappers were working.

## For Project Managers

### Before Fieldwork

- Ask local partners whether interference has been reported in the area.
- Provide offline imagery (basemaps) so mappers can work without GPS.
- Brief the team on the 10-second check and on manual placement.
- In QField projects, agree on accuracy settings with the team.

### During Fieldwork

- Ask mappers to report when the dot is wrong, with time and rough place.
- Several reports from one area on one day usually mean interference, not
  user error.
- Do not push mappers to keep moving around to "find signal".

### After Fieldwork: Checking Submissions

Features selected from the map are usually still correct even when the
phone's own position was wrong. Points captured from GPS need a closer look.

For ODK projects, download the submissions from ODK Central as CSV. Each
location field is split into four columns, for example `warmup-Latitude`,
`warmup-Longitude`, `warmup-Altitude` and `warmup-Accuracy`. Patterns worth
reviewing:

- **Three or more submissions with exactly the same coordinates**, often from
  different devices: spoofing, or a cached position. Real GPS readings almost
  never repeat to the metre.
- **A position far from where most submissions were recorded**: spoofing, or
  the form was filled in elsewhere.
- **A mapper's consecutive submissions too far apart for the time between
  them**: spoofing.
- **Accuracy of exactly 0 m**: often a simulated or injected position.
- **Accuracy of tens or hundreds of metres**: jamming, or a very short time to
  get a fix.
- **No position recorded at all**: jamming, if several occur together.

!!! note

    These patterns are hints for a human review, not proof. Look at each
    flagged submission, ideally together with the mapper, before you correct
    or remove anything.
