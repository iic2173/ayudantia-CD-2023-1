# Ayudantia-CD-2023-1

En este repositorio aprenderán  1 sobre Continuous Deployment (CD) de frontend en un bucket de S3 de AWS con Circle CI.

---

## Video cápsula teórica:
[https://www.youtube.com/watch?v=37MTH06EbE0](https://www.youtube.com/watch?v=37MTH06EbE0)

---

## Guía sobre como hacer un deploy automático de app en AWS con Circle Ci

- Primero creamos el bucket público en S3, con el "getObject" permitido en la política de bucket y "Static Website Hosting" habilitado en las propiedades.
-  Luego, podemos [instalar la CLI de AWS](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) en nuestro computador para probar localmente que funcione la sincronización con el bucket S3. Recordemos que podemos revisar si se instaló correctamente con `aws --version` y configurar las credenciales con `aws configure` ("Credenciales de Seguridad > Claves de acceso para CLI > Crear una clave de acceso").
-  Ahora, ya podemos generar la carpeta build de nuestra app con `yarn build` para luego enviarla al bucket S3 con `aws s3 sync build s3://<nombre_bucket_S3> --delete` (recordar que el flag `--delete` no es necesario, pero sirve para remover archivos en la sincronización en caso de que fuera necesario).
-  Ya estamos listos para automatizar este proceso, agregándolo al final del workflow de circleci (revisar configuración en archivo `.circleci/config.yml` y los "scripts" agregados en `frontend/package.json`). No olvidar que debemos agregar las credenciales de AWS para la CLI en las variables de entorno de circleci con los nombres "AWS_ACCESS_KEY_ID" y "AWS_SECRET_ACCESS_KEY". Estos datos **nunca** deben insertarse directamente en el código por riesgo de seguridad.
-  Por último, con lo anterior funcionando, agregaremos Cloudfront al deploy de nuestra aplicación: crear la distribución asociada al bucket S3, con nuevo "Origin access control settings", activando la opción de "redirección HTTP a HTTPS" y definiendo el "Default Root Object" como `index.html`.
-  Para más información, los invitamos a revisar este ([ejemplo de CD similar](https://blog.cloudboost.io/react-with-circleci-aws-s3-and-aws-cloudfront-844a1b2c75c9)). ¡Mucho éxito con el resto de la entrega! 💪💪