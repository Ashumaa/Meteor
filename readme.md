TO-DO LIST
-------
In questa applicazioni abbiamo una to-do list, che tramite mongoDB salva le cose da fare degli utenti registrati, hostata da [Galaxy](https://www.meteor.com/cloud), un servizio offerto da meteor.


In questo template vengono aggiunti anche pacchetti aggiuntivi di meteor che si possono trovare su un altro servizio che Meteor fornisce:
[Atmosphere]( https://atmospherejs.com).
Alcuni pacchetti aggiunto sono simpl-schema e percolate:migrations, il primo è utilizzato per validare gli schema durante il run-time mentre il secondo crea le migrazioni del database.
```
import { Meteor } from "meteor/meteor";
import { Migrations } from "meteor/percolate:migrations";
import "./db/migrations";
import "./tasks/tasks.methods";
import "./tasks/tasks.publications";


Questo è l'entry point del server

Meteor.startup(() => {
  Migrations.migrateTo("latest");
});
```
Raccoglie tutte le migrazioni che non sono state applicate e le applica, un buon utilizzi per esempio e quando c'è una modifica nel DB e hai bisogno che ogni persona abbia i dati di default.

Gli schema vengono utilizzati in modo che i dati che arrivano all'utente(frontend) siano come c'è li aspettiamo.

Connessione al server
------------
Meteor lavora in modo simile
```
/**
 * Rimuove una task.
 * @param {{ taskId: String }}
 * @throws un errore quando non si è loggati
 */
export const removeTask = ({ taskId }) => {
  checkTaskOwner({ taskId });
  TasksCollection.remove(taskId);
};
...
Meteor.methods({
  insertTask,
  removeTask,
  toggleTaskDone,
});
```
