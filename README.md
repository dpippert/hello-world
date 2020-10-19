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

<table>
  <thead>
    <tr>
      <th cols="3">Game</th>
    </tr>
    <tr>
      <th>Field</th><th>Type</th><th>Notes</th>
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
      <td>ascore</td>
      <td>Integer</td>
      <td>Away team final score</td>
    </tr>
    <tr>
      <td>hscore</td>
      <td>Integer</td>
      <td>Home team final score</td>
    </tr>
    <tr>
      <td>date</td>
      <td>Date</td>
      <td>Only mm/dd/yyyy needed</td>
    </tr>
  </tbody>
</table>

