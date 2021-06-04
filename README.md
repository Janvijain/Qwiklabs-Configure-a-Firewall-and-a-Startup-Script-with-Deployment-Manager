# Qwiklabs Configure a Firewall and a Startup Script with Deployment Manager

Cloude Shell                     
mkdir deployment_manager                       
cd deployment_manager                           
gsutil cp gs://spls/gsp302/* .                          

Open Editor                                  
 
Delete all the content of qwiklabs.jinja and paste                           

resources:   
- name: default-allow-http                               
  type: compute.v1.firewall                             
  properties:                              
    targetTags: ["http"]                               
    sourceRanges: ["0.0.0.0/0"]                            
    allowed:                              
      - IPProtocol: TCP                                
        ports: ["80"]                                
- type: compute.v1.instance                            
  name: vm-test                        
  properties:                         
    zone: {{ properties["zone"] }}                       
    machineType: https://www.googleapis.com/compute/v1/projects/{{ env["project"] }}/zones/{{ properties["zone"] }}/machineTypes/f1-micro                      
    #For examples on how to use startup scripts on an instance, see:                               
    #https://cloud.google.com/compute/docs/startupscript                 
    tags:                             
        items: ["http"]                    
    metadata:                       
      items:                    
      - key: startup-script                    
        value: "apt-get update \n apt-get install -y apache2"             
    disks:                         
    - deviceName: boot                         
      type: PERSISTENT                          
      boot: true                       
      autoDelete: true                            
      initializeParams:                              
        diskName: disk-{{ env["deployment"] }}                    
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/family/debian-9                             
    networkInterfaces:                                                                                               
    - network: https://www.googleapis.com/compute/v1/projects/{{ env["project"] }}/global/networks/default                                        
      #Access Config required to give the instance a public IP address                                                                
      accessConfigs: 
      -name: External NAT
       type: ONE_TO_ONE_NAT
                                             
To save press Ctrl+S                                                   

To deploy requirements                              
Cloud Shell                          
gcloud deployment-manager deployments create deployment-templates --config qwiklabs.yaml                

Navigation Menu -> Compute Engine ->VM Instances ->vm-test ->Edit                    
Firewalls                                 
Check Allow HTTP traffic                                
Save                           

Click on External IP to view Apache page                     

Check My Progress                     

# Congratulations! you have successfully completed your lab                       

LinkedIn                                                             
https://www.linkedin.com/in/janvi-shree-shrimal-9433401aa/                            

Twitter                           
https://twitter.com/janvissm                            

Qwiklabs Profile                         
https://run.qwiklabs.com/public_profiles/5c709a0a-adf5-4f7d-9e9d-9f71d6c49c6e                     

Medium                            
https://janvissm.medium.com/                            


