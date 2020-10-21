## Sports Wagering Database Proposal

### Team members

- Christian Wettre
- Mahima Chandan
- Dale Pippert
- Amrutha Ravi
- Madeline Rys

### GitHub URL

https://github.com/madelinerys/CS546-Final-Project

<h2 id="games">Games collection</h2>

Each document describes an NFL game, capturing only those few aspects of
a game that are relevant to the system, such as its start date and time,
and final score.  An NFL season is 17 weeks with 14 games per week, so
this collection for a full season would have 17 * 14 = 238 documents in
it. Each week, another 14 games are added to the table. Alternatively,
it could be seeded with all 238 games at once. Either way an automated
job is responsible for populating this collection.

#### Games schema

<table>
  <thead>
    <tr>
      <th>Field</th><th>Type</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>_id</td>
      <td>ObjectId</td>
      <td></td>
    </tr>
    <tr>
      <td>away</td>
      <td>String</td>
      <td>designator for away (visiting) team</td>
    </tr>
    <tr>
      <td>home</td>
      <td>String</td>
      <td>designator for home team</td>
    </tr>
    <tr>
      <td>week</td>
      <td>Integer</td>
      <td>Week of the season, 1-17. NFL weeks start on Tuesday and end on
      Monday. As a frame of reference, 10/21/2020 is in Week 7. If you want
      to know, for example, what is the _current_ _week_ right now_ as a frame
      of reference you can go to https://www.espn.com/nfl/schedule and it
      should default to bring up the current week.</td>
    </tr>
    <tr>
      <td>start</td>
      <td>Date</td>
      <td>Game start date and time GMT. The system will not allow bets to be
      placed on any game where **start** is >= current date and time. Upcoming
      games must be inserted into this table at least one week prior to *start*,
      so that bettors have a chance to place their wagers on the game.</td>
    </tr>
    <tr>
      <td>ascore</td>
      <td>Integer</td>
      <td>Away team final score. Null up until when the game has finished and the
      system has processed the final score feed.</td>
    </tr>
    <tr>
      <td>hscore</td>
      <td>Integer</td>
      <td>Home team final score. Null up until when the game has finished and the
      system has processed the final score feed.</td>
    </tr>
  </tbody>
</table>

#### Games example document

```
{
  _id: "5f8578c116e3409b5b276d50",
  away: "TEX",
  home: "TIT",
  week: 6,
  date: "2020-10-18T17:00:00.000Z"
  ascore: 36,
  hscore: 42,
}
```

#### Games notes

1. GMT is four hours ahead of EDT, and five hours ahead of EST. For example,
1:00 PM EDT is 5:00 PM GMT.

<h2 id="teams">Teams collection</h2>

A seeded reference collection to store static information for all 32 NFL teams. This
collection has exactly 32 documents in it, one per team.

#### Teams schema

<table>
  <thead>
    <tr>
      <th>Field</th><th>Type</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>_id</td>
      <td>ObjectId</td>
      <td></td>
    </tr>
    <tr>
      <td>abbrev</td>
      <td>Three-letter unique business key</td>
      <td>Enhances readability</td>
    </tr>
    <tr>
      <td>fran</td>
      <td>String</td>
      <td>Franchise name, typically locale/city/state of team</td>
    </tr>
    <tr>
      <td>nn</td>
      <td>String</td>
      <td>Team nickname</td>
    </tr>
  </tbody>
</table>

#### Teams example document

```
{
  _id: "5f85808c9dad05d358aae011",
  abbrev: "TIT",
  fran: "Tennessee",
  nn: "Titans"
}
```

<h2 id="lines">Lines collection</h2>

Stores betting lines for each game. The system populates this collection on a week-by-week,
day-by-day basis. It is updated with new lines every day or two via a background job.

#### Lines schema

<table>
  <thead>
    <tr>
      <th>Field</th><th>Type</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>_id</td>
      <td>ObjectId</td>
      <td></td>
    </tr>
    <tr>
      <td>gameid</td>
      <td>ObjectId</td>
      <td>_id from Games collection</td>
    </tr>
    <tr>
      <td>ltype</td>
      <td>String</td>
      <td>AML|HML|ASP|HSP|OV|UN (see notes)</td>
    </tr>
    <tr>
      <td>num</td>
      <td>Integer</td>
      <td>The number, the meaning of which depends on ltype; may be positive, negative, or 0</td>
    </tr>
    <tr>
      <td>date</td>
      <td>Date</td>
      <td>mm/dd/yyyy</td>
    </tr>
  </tbody>
</table>

#### Lines example document

```
{
  _id: "5f85808c9dad05d358aae00a",
  gameid: "5f8578c116e3409b5b276d50",
  ltype: "ASP",
  num: -2,
  date: "10/16/2020"
}
```

#### Lines notes

1. There can be many documents for any given gameid and ltype.

1. For any given gameid and ltype, the current line for that gameid
and ltype is the line with the most recent date.

1. For any given gameid and ltype, bets are always placed against
the line having the most recent date.

1. Meaning of num depends on ltype as follows.

    1. **AML** Away team money line.

    1. **HML** Home team money line.

    1. **ASP** Away team spread.
    
    1. **HSP** Home team spread.
    
    1. **OV** Over points.

    1. **UN** Under points.

<h2 id="bets">Bets collection</h2>

Records and stores bets. Each document is a bet from a bettor aka user. The
user interface is used to enter bets.

#### Bets schema

<table>
  <thead>
    <tr>
      <th>Field</th><th>Type</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>_id</td>
      <td>ObjectId</td>
      <td></td>
    </tr>
    <tr>
      <td>bettorid</td>
      <td>ObjectId</td>
      <td>_id of the bettor</td>
    </tr>
    <tr>
      <td>lineid</td>
      <td>ObjectId</td>
      <td>_id of the line</td>
    </tr>
    <tr>
      <td>amount</td>
      <td>Number</td>
      <td>Dollar amount of the bet</td>
    </tr>
    <tr>
      <td>paid</td>
      <td>Number</td>
      <td>Dollars this bet paid, or null if still live</td>
    </tr>
    <tr>
      <td>enter</td>
      <td>Date</td>
      <td>Time and Date of the bet</td>
    </tr>
    <tr>
      <td>end</td>
      <td>Date</td>
      <td>Time and Date the bet resolved</td>
    </tr>
  </tbody>
</table>

#### Bets example document

````
{
  _id: "3e85908c9dad05d2589ae104",
  bettorid: "4e9441Cc9dad05d268aff018", 
  lineid: "8015617daebe1773199ec12C",
  amount: 50,
  paid: null,
  enter: "10/20/2020",
  end: null
}
````

<h2 id="bettors">Bettors collection</h2>

These are users aka bettors that have signed up. Possibly (time permitting) seeded with
1000 bettors for demo purposes.

<table>
  <thead>
    <tr>
      <th>Field</th><th>Type</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>_id</td>
      <td>ObjectId</td>
      <td></td>
    </tr>
    <tr>
      <td>username</td>
      <td>String</td>
      <td></td>
    </tr>
    <tr>
      <td>pwd</td>
      <td>String</td>
      <td>Hashed password</td>
    </tr>
    <tr>
      <td>balance</td>
      <td>Number</td>
      <td>Dollar balance in account</td>
    </tr>
</tbody>
</table>

1. Surprisingly, this collection will be seeded with exactly the same 1000 people
as are present in an earlier people.json lab. They all like to bet.
