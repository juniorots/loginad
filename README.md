# PROVA CONCEITO DE INTEGRACAO MICROSOFT AZURE LDAP

## Consumo de imagens Docker para o host
'''
sudo docker run --hostname demoldap.eastus.cloudapp.azure.com --name demoldap -v $(pwd)/ldap-user:/container/service/slapd/assets/test --detach osixia/openldap:1.5.0
sudo docker cp demoldap:/container/service/slapd/assets/certs certs
sudo docker container exec demoldap ldapadd -H ldap://demoldap.uksouth.cloudapp.azure.com -D "cn=admin,dc=demo,dc=com" -w admin -f /container/service/slapd/assets/test/user.ldif
'''

## Configuração do serviço no Azure Cloud
'''
ldap_search_base_distinguished_name='dc=demo,dc=com'
ldap_server_certificates='/usr/csuser/clouddrive/ldap.crt'
ldap_server_hostname='demoldap.eastus.cloudapp.azure.com'
ldap_service_user_distinguished_name='cn=admin,dc=demo,dc=com'
ldap_service_user_password='temp'
'''

'az managed-cassandra datacenter update -g `NetworkWatcherRG` -c `demo` -d datacenter-1 --ldap-search-base-dn $ldap_search_base_distinguished_name --ldap-server-certs $ldap_server_certificates --ldap-server-hostname $ldap_server_hostname --ldap-service-user-dn $ldap_service_user_distinguished_name --ldap-svc-user-pwd $ldap_service_user_password'

## Havendo inconsistências com o Subnet, devido ausência de hole para Microsoft Cosmo DB, tente tratar com o seguinte trecho:
'az role assignment create --assignee a232010e-820c-4083-83bb-3ace5fc29d0b --role "Network Contributor" --scope /subscriptions/345818eb-9a85-404b-95b2-f7bda1faca29/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/virtualNetworks/demo-net/subnets/subdemo'


