# boneio-loxone-via-udp-http
How to integrate BoneIO with Loxone via UDP & HTTP

For getting info from BoneIO into Loxone we use UDP connection
For sending commands from Loxone into BoneIO we use HTTP connection. 

Note: We could use UDP however it would require additional configuration changes in the yaml, where HTTP commands are working out of the box.

## 0. Download files
### 0.1. Go to github repository and download the zip just like that:

![image](https://github.com/user-attachments/assets/94b8d49c-6d91-4ca6-af7c-9ff73deaf68d)

### 0.2. Save it to the location you want on your desktop (preferrably `Downloads` directory)

## 1. Adjust yaml config
### 1.1. Open `boneio-dr-8ch-loxone.yaml.yaml` in some kind of editor
### 1.2. Update fields
- `static_ip_address`: static ip address in your network that the boneIO will be attached to
- `loxone_ip`: Miniserver's IP address
- `loxone_udp_port`: UDP port that BoneIO will send data to (if using multiple devices, you might want to change this)

![image](https://github.com/user-attachments/assets/628f7fa1-6ad5-4a75-bff1-a638898d66c3)

### 1.3. Save changes in a file

## 2. Update esp32 configuration on your boneIO device
There are multiple ways to do it: via usb cable, network using esphome application

![image](https://github.com/user-attachments/assets/c75c41ee-8efa-456c-9dbc-775f1e054bcd)

- Go to `esphome` application
- Find your device and click `Edit`

![image](https://github.com/user-attachments/assets/a86256df-6a80-4177-b391-7ce38851b8f3)

- Click `Install` and wait until it completes

![image](https://github.com/user-attachments/assets/2058e19d-c60f-4503-96a7-49ad2a9a6a23)

## 3. Loxone: Configure VI/VO
### 1. Open your project in Loxone Config
### 2. Virtual Inputs: 
#### 2.1. Import Virtual Inputs for template
- Find and Left Click on `Virtual Inputs` under your Miniserver

![image](https://github.com/user-attachments/assets/b630367a-bca5-480b-8aca-5cc0844665a5)

- Import Template by clicking: UDP Device Template -> Import Template... -> Find the file downloaded earlier `VIU_BoneIO Dimmer 8ch 8di.xml`

![image](https://github.com/user-attachments/assets/f0db4d49-bd27-4def-8144-d119bb68e405)

- Use newly created template by clicking: UDP Device Template -> Import Template... -> Choose under `My Templates:` `VIU_BoneIO Dimmer 8ch 8di.xml`

#### 2.2. Update UDP receive port on newly created Virtual Input to boneIO's ip address
Change `UDP receive port` to the one from `.yaml` config under `loxone_udp_port` (defaults to `9999` and no changes are required)

![image](https://github.com/user-attachments/assets/88162506-edf8-460a-ab51-56b4488d737f)

### 3. Virtual Outputs
#### 3.1. Import Virtual Outputs from template:
- Find and Left Click on `Virtual Outputs` under your Miniserver

![image](https://github.com/user-attachments/assets/c095a468-4b75-4369-9a51-40cdcb07fcc5)

- Import Template by clicking: Device Templates -> Import Template... -> Find the file downloaded earlier `VO_BoneIO Dimmer 8ch 8di.xml`

![image](https://github.com/user-attachments/assets/fa479c0f-1713-4f96-9634-c2f699cd6cb5)

- Use newly created template by clicking: `Device Templates` -> `Import Template...` -> Choose: `VO_BoneIO Dimmer 8ch 8di.xml`

![image](https://github.com/user-attachments/assets/bd540715-09e6-43e3-ab2c-92b0ff72ba12)

#### 3.2. Update Address field on newly created Virtual Output to boneIO's ip address
Change `Address` to the one from `.yaml` config under `static_ip_address` field

![image](https://github.com/user-attachments/assets/4f334829-3454-4cd2-b35e-0cc0fd03abf3)

### 4. Loxone-config
#### 4.1. How to use Virtual Inputs:

![image](https://github.com/user-attachments/assets/e191fe7d-158e-4d3c-aad6-36361c3a4706)

- `DI1-8`: Digital inputs. Possible values: `0`, `1`
- `CHL Power`/`CHR Power`: Power in watts for Left (CHL) and Right (CHR) channels
- `KeepAliveTime`: UnixTimestamp of last ping from BoneIO to Miniserver.
In order to make a use of it we can use `Formula`: and Memory Flag: `BoneIO Online` as such:

![image](https://github.com/user-attachments/assets/bfddcd08-f44d-4bf3-b064-8a1829568fe3)

1. Create new Memory Flag: `BoneIO Online`
2. Add formula block with: `IF(I2 - I1 > 30;0;1)`
3. Connect `KeepAliveTime` to I1 of Formula block
4. Connect `UnixTimestamp` to I2 of Formula block
5. Connect `R` from Formula block into newly created Memory Flag: `BoneIO Online`

#### 4.2. How to use Virtual Outputs

![image](https://github.com/user-attachments/assets/8c2613ba-098a-464d-9ef9-d1a1634b9867)

- `CHL1-CHL4`/`CHR1-CHR4`: Relays for Left (CHL1-4) and Right Channels (CHR1-4). Possible values to send: `0`, `1`
- `CHL1 Dimmer`-`CHL4 Dimmer`/`CHR1 Dimmer`-`CHR4 Dimmer`: Dimmers for Left (CHL1-4) and Right Channels (CHR1-4). Possible values to send: `0-255`

Above virtual output commands can be used in `Lighting Controller` as such:
![image](https://github.com/user-attachments/assets/b1bf52b8-7c29-4495-b685-c0f066789083)


