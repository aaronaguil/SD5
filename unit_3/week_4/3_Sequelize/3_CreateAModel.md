### Create a Model
* A Sequelize model is very similar to a Java Entity
  
* Create all model definitions in the 'models' directory
  
* The file naming convention is the singular lowercased name of the model. (i.e. User => user.js, Todo => todo.js)
  
* Create a 'user.js' file and define a User model within it:
  
```bash
> touch models/user.js
```
  
* Now create the model inside of an exported function. This function will be called auto-magically by 'index.js' and added to the list of models it makes available:
  
```javascript
// In models/user.js

module.exports = function(sequelize, DataTypes) {
	var User = sequelize.define("User", {
		username : DataTypes.STRING
	}, {
		classMethods: {
			associate : function(models) {
				User.hasMany(models.Task)
			}
		}
	});

	return User;
}
```
  
* The above model defines the schema for Sequelize to use. 
  
* The `Model.define(name, {configuration})` method sets the schema for your database.
  
* The above model ('User'), denotes a 'hasMany' relationship with a model named 'Task', let's create Task now.
  
```bash
> touch models/task.js
```
  
```javascript
// In models/task.js
module.exports = function(sequelize, DataTypes) {
	var Task = sequelize.define("Task", {
		title : DataTypes.STRING
	}, {
		classMethods: {
			associate : function(models) {
				Task.belongsTo(models.User, {
					onDelete : "CASCADE",
					foreignKey : {
						allowNull : false // must be associated
					}
				});
			}
		}
	});

	return Task;
}
```
  
* Below is a full list of Sequelize datatypes available and their rough SQL equivalent

```javascript
Sequelize.STRING                      // VARCHAR(255)
Sequelize.STRING(1234)                // VARCHAR(1234)
Sequelize.STRING.BINARY               // VARCHAR BINARY
Sequelize.TEXT                        // TEXT
Sequelize.TEXT('tiny')                // TINYTEXT

Sequelize.INTEGER                     // INTEGER
Sequelize.BIGINT                      // BIGINT
Sequelize.BIGINT(11)                  // BIGINT(11)

Sequelize.FLOAT                       // FLOAT
Sequelize.FLOAT(11)                   // FLOAT(11)
Sequelize.FLOAT(11, 12)               // FLOAT(11,12)

Sequelize.REAL                        // REAL        PostgreSQL only.
Sequelize.REAL(11)                    // REAL(11)    PostgreSQL only.
Sequelize.REAL(11, 12)                // REAL(11,12) PostgreSQL only.

Sequelize.DOUBLE                      // DOUBLE
Sequelize.DOUBLE(11)                  // DOUBLE(11)
Sequelize.DOUBLE(11, 12)              // DOUBLE(11,12)

Sequelize.DECIMAL                     // DECIMAL
Sequelize.DECIMAL(10, 2)              // DECIMAL(10,2)

Sequelize.DATE                        // DATETIME for mysql / sqlite, TIMESTAMP WITH TIME ZONE for postgres
Sequelize.DATE(6)                     // DATETIME(6) for mysql 5.6.4+. Fractional seconds support with up to 6 digits of precision 
Sequelize.DATEONLY                    // DATE without time.
Sequelize.BOOLEAN                     // TINYINT(1)

Sequelize.ENUM('value 1', 'value 2')  // An ENUM with allowed values 'value 1' and 'value 2'
Sequelize.ARRAY(Sequelize.TEXT)       // Defines an array. PostgreSQL only.

Sequelize.JSON                        // JSON column. PostgreSQL only.
Sequelize.JSONB                       // JSONB column. PostgreSQL only.

Sequelize.BLOB                        // BLOB (bytea for PostgreSQL)
Sequelize.BLOB('tiny')                // TINYBLOB (bytea for PostgreSQL. Other options are medium and long)

Sequelize.UUID                        // UUID datatype for PostgreSQL and SQLite, CHAR(36) BINARY for MySQL (use defaultValue: Sequelize.UUIDV1 or Sequelize.UUIDV4 to make sequelize generate the ids automatically)

Sequelize.RANGE(Sequelize.INTEGER)    // Defines int4range range. PostgreSQL only.
Sequelize.RANGE(Sequelize.BIGINT)     // Defined int8range range. PostgreSQL only.
Sequelize.RANGE(Sequelize.DATE)       // Defines tstzrange range. PostgreSQL only.
Sequelize.RANGE(Sequelize.DATEONLY)   // Defines daterange range. PostgreSQL only.
Sequelize.RANGE(Sequelize.DECIMAL)    // Defines numrange range. PostgreSQL only.

Sequelize.ARRAY(Sequelize.RANGE(Sequelize.DATE)) // Defines array of tstzrange ranges. PostgreSQL only.

Sequelize.GEOMETRY                    // Spatial column.  PostgreSQL (with PostGIS) or MySQL only.
Sequelize.GEOMETRY('POINT')           // Spatial column with geomerty type.  PostgreSQL (with PostGIS) or MySQL only.
Sequelize.GEOMETRY('POINT', 4326)     // Spatial column with geomerty type and SRID.  PostgreSQL (with PostGIS) or MySQL only.
```
  
#### Continue to [4 - Querying Data](4_QueryingData.md)