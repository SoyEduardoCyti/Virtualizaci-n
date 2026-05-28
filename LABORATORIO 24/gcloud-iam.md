# Paso 3 - Política IAM del proyecto brave-aileron-497301-i8
# Comando: gcloud projects get-iam-policy brave-aileron-497301-i8 --filter="bindings.role:roles/viewer"
# Usuario: angel@angel-VMware-Virtual-Platform

---
bindings:
- members:
  - serviceAccount:808952593958@cloudservices.gserviceaccount.com
  role: roles/compute.instanceGroupManagerServiceAgent
- members:
  - serviceAccount:service-808952593958@compute-system.iam.gserviceaccount.com
  role: roles/compute.serviceAgent
- members:
  - serviceAccount:808952593958-compute@developer.gserviceaccount.com
  role: roles/editor
- members:
  - user:19292901@uagro.mx
  role: roles/owner
- members:
  - serviceAccount:auditor-plataformas@brave-aileron-497301-i8.iam.gserviceaccount.com
  role: roles/viewer
etag: BwZS6MNmdRQ=
version: 1

# Conclusión:
# Es peligroso usar la llave privada del administrador principal en el código fuente
# porque si el repositorio es público o es comprometido, un atacante obtiene acceso
# total al proyecto con permisos de Owner, pudiendo borrar, modificar o robar
# todos los recursos y datos sin restricción alguna.