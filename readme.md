TO-DO LIST
-------
In questa applicazioni abbiamo una to-do list, che tramite mongoDB salva le cose da fare degli utenti registrati, hostata da [Galaxy](https://www.meteor.com/cloud), un servizio offerto da meteor.


In questo template vengono aggiunti anche pacchetti aggiuntivi di meteor che si possono trovare su un altro servizio che Meteor fornisce:
[Atmosphere]( https://atmospherejs.com).
Alcuni pacchetti aggiunto sono simpl-schema e percolate:migrations, il primo è utilizzato per validare gli schema durante il run-time mentre il secondo crea le migrazioni del database.
```
//api/main.js
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
Meteor lavora in modo simile [tRPC](https://trpc.io) e [Blitz.js](https://blitzjs.com) (altre tecnologie utilizzate per la connessione server)
```
//api/tasks/tasks.methods.js
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
Per chiamare questo metodo del server dobbiamo usare il suo nome, esempio:
```
//ui/tasks/TaskItems.jsx:
 onDelete={taskId => Meteor.call('removeTask', { taskId })}
```

Metodi utilizzati nel progetto
------
```
Meteor.methods(methods), definisce funzioni del server che possono essere invocate dal client

this.userId, ritorna l'id dell'utente se nessuno è loggato ritorna null(id utilizzato per identificare l'utente nel database)

new Meteor.Error(error, [reason], [details]), rappresente un errore lanciato da un metodo

new Mongo.Collection(name, [options]), crea una collection (tipo di oggetto che rappresenta 
un insieme di documenti all'interno del database, facilemte accessibile e gestibile)

gli oggetti Collection hanno metodi come .insert .remove .findOne etc.

Meteor.startup(func), lancia il codice quando un client o un server parte

Meteor.loginWithPassword(selector, password, [callback]), Logga l'utente con una password

Meteor.logout([callback]), Disconnette l'utente
```
