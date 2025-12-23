## Arquitectura
Esta aplicación de Spring Boot utiliza tanto controladores MVC como REST. Se utilizan plantillas de Thymeleaf para los paneles de administración y de doctor, mientras que las API REST sirven a todos los demás módulos. La aplicación interactúa con dos bases de datos: MySQL (para datos de pacientes, doctores, citas y administración) y MongoDB (para recetas). Todos los controladores dirigen las solicitudes a través de una capa de servicio común, que a su vez delega en los repositorios apropiados. MySQL utiliza entidades JPA mientras que MongoDB utiliza modelos de documentos.

## Flujo de datos:
1. User accede al AdminDashboard o a la pagina de Appointments.
2. La accionm es enrutada por el Thymeleaf o el controlador REST apropiado.
3. El controllador llama al Service Layer.
4. Dependiendo del servicio, se usa MySQL Repositories o MongoDB Repository.
5. MYSQL Repositories accede a MYSQL Database, mientras que MongoDB Repository accede a MongoDB Database.
6. Se acceden a los modelos correspondientes de MySQL, o MongoDB segun el caso.
7. De MySQL, mediante las entidades JPA para datos estructurados se puede acceder a Patient, Doctor, Appointment y Admin. Mientras que por el lado de MongoDB se accede a la Prescription que es el modelo de MongoDB Models para datos dinamicos.
8. 
