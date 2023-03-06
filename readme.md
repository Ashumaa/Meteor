In questa applicazioni abbiamo una to-do list, che tramite mongoDB salva le cose da fare degli utenti registrati, hostata da Galaxy, un servizio offerto da meteor.
https://www.meteor.com/cloud
----------------------------
In questo template vengono aggiunti anche pacchetti aggiuntivi di meteor che si possono trovare su un altro servizio che Meteor fornisce:
Atmosphere, https://atmospherejs.com.
Alcuni pacchetti aggiunto sono simpl-schema e percolate:migrations, il primo è utilizzato per validare gli schema durante il run-time mentre il secondo crea le migrazioni del database.
```
import { Meteor } from "meteor/meteor";
import { Migrations } from "meteor/percolate:migrations";
import "./db/migrations";
import "./tasks/tasks.methods";
import "./tasks/tasks.publications";


This is the server-side entry point

Meteor.startup(() => {
  Migrations.migrateTo("latest");
});
```
