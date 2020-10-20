## Sports Wagering Database Proposal

### Team members

- Christian Wettre
- Mahima Chandan
- Dale Pippert
- Amrutha Ravi
- Madeline Rys

### GitHub URL

https://github.com/madelinerys/CS546-Final-Project

Contests in sports come in various shapes and sizes. The only type of contest
supported by our first phase delivery is a <a href=#game>game</a>. A game is
modeled in Mongo as depicted in Table TBD below.

#### Games collection
Each document is an NFL game, capturing only those aspects of a game that are
relevant to the system. An NFL season is 17 weeks with 14 games per week, so this
collection for a full season would have 17 * 14 = 238 documents in it.

##### Games schema

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
      <td>designator for away (visiting) team</td>
      <td></td>
    </tr>
    <tr>
      <td>home</td>
      <td>designator for home team</td>
      <td></td>
    </tr>
    <tr>
      <td>week</td>
      <td>Integer</td>
      <td>Week of the season, 1-17</td>
    </tr>
    <tr>
      <td>date</td>
      <td>Date</td>
      <td>Only mm/dd/yyyy needed</td>
    </tr>
    <tr>
      <td>ascore</td>
      <td>Integer</td>
      <td>Away team final score</td>
    </tr>
    <tr>
      <td>hscore</td>
      <td>Integer</td>
      <td>Home team final score</td>
    </tr>
  </tbody>
</table>

##### Games example document

```
{
  _id: "5f8578c116e3409b5b276d50",
  away: "TEx"
  home: "TIT"
  week: 6
  date: 10/18/2020
  ascore: 36
  hscore: 42
}
```

##### Notes

1. A null ascore or hscore indicates a game that has either not yet played, is currently 
in progress, or has completed but the score has not yet been captured by the system.

#### Teams collection
Seeded with all 32 NFL Teams.

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

<h2 id="lines">Lines collection</h2>

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
      <td>AML|HML|ASP|HSP|OV|UN</td>
      <td></td>
    </tr>
    <tr>
      <td>thenum</td>
      <td></td>
      <td>your number</td>
    </tr>
    <tr>
      <td>date</td>
      <td>Date</td>
      <td>mm/dd/yyyy</td>
    </tr>
  </tbody>
</table>

1. There can be many documents for any given gameid and ltype.

1. For any given gameid and ltype, the current line for that gameid
and ltype is the line with the most recent date.

1. For any given gameid and ltype, bets are always placed against
the line having the most recent date.

1. Meaning of thenum depends on ltype as follows.

    a. **AML** Away team money line.

    a. **HML** Home team money line.

    a. **ASP** Away team spread.
    
    a. **HSP** Home team spread.
    
    a. **OV** Over points.

    a. **UN** Under points.

#### Bets collection

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
      <td>_id of the bettor from the [Bettors]: #bettors collection</td>
    </tr>
    <tr>
      <td>lineid</td>
      <td>_id of the line from the [Lines](#lines) collection</td>
      <td>Enhances readability</td>
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
      <td>date</td>
      <td>Date</td>
      <td>Time and Date of the bet</td>
    </tr>
    <tr>
      <td>paydate</td>
      <td>Date</td>
      <td>Time and Date of bet paid</td>
    </tr>
    <tr>
      <td>nn</td>
      <td>String</td>
      <td>Team nickname</td>
    </tr>
  </tbody>
</table>

#### Codes collection

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

<h4 id="bettors">Bettors collection</h2>

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
