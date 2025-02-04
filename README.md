# boneio-loxone-via-udp
How to integrate BoneIO with Loxone via UDP

# 1. Update yaml config with your ip addresses etc

# 2. Update esp32 configuration
Yaml using usb cable or esphome application

# 3. Loxone
## 1. Open your project in Loxone Config
## 2. Virtual Inputs: 
### 2.1. Import Virtual Inputs for template
`Go to Virtual Inputs -> UDP Device Template -> Import Template... -> Choose: VIU_BoneIO Dimmer 8ch 8di.xml`
### 2.2. Update UDP receive port on newly created Virtual Input to boneIO's ip address
## 3. Virtual Outputs
### 3.1. Import Virtual Outputs from template:
`Go to Virtual Outputs -> Device Templates -> Import Template... -> Choose: VO_BoneIO Dimmer 8ch 8di.xml`
### 3.2. Update Address field on newly created Virtual Output to boneIO's ip address
