## Checkpoint 0
Blog link: https://github.com/jweiss0/oss-repo-template/wiki/Term-Project-Progress-Updates#part-2-lab-discussion

## Checkpoint 1
Browser at `http://localhost:5984`:
![01-01](https://user-images.githubusercontent.com/18493608/161302899-743e8224-e689-475d-a14d-fa53358313e0.png)

## Checkpoint 2
#### Getting Started Part 1
![02-01](https://user-images.githubusercontent.com/18493608/161302914-783c6953-28f6-4ff8-9a0f-638ecd023047.png)

#### Getting Started Part 2
![02-02](https://user-images.githubusercontent.com/18493608/161302925-7784d02b-1c27-4b0a-aee2-0f61bc5505cb.png)
![02-03](https://user-images.githubusercontent.com/18493608/161302952-9ca9ce63-ec12-4b86-86ce-8d3103bf5d75.png)
![02-04](https://user-images.githubusercontent.com/18493608/161302961-0fb343bf-0dce-435d-9f6a-613aadc6ef4e.png)
![02-05](https://user-images.githubusercontent.com/18493608/161302967-362fa2de-39f5-4a18-add5-3aa72bd26ea4.png)
![02-06](https://user-images.githubusercontent.com/18493608/161302973-12d0bf3c-41c2-478a-b106-05c168583481.png)
![02-07](https://user-images.githubusercontent.com/18493608/161302981-158027ed-b91c-483a-bc03-dbe25701f945.png)

## Checkpoint 3
Creating `albums` and `albums-backup` databases:
![03-01](https://user-images.githubusercontent.com/18493608/161302987-ca3a4c20-2fc3-4698-bd97-5a1120742096.png)
Deleting `albums-backup` database:
![03-02](https://user-images.githubusercontent.com/18493608/161303001-1a25d7fd-897f-445a-9aa1-1cd0a92f2a3d.png)
Creating and getting document:
![03-03](https://user-images.githubusercontent.com/18493608/161303015-5dbba399-78ff-47a1-8929-317a4594852b.png)
Updating document:
![03-04](https://user-images.githubusercontent.com/18493608/161303025-a35540c0-eade-4026-bffe-cd9e856b2d02.png)
Adding new document (verbose):
![03-05](https://user-images.githubusercontent.com/18493608/161303050-e674e4fa-37da-461d-9956-15d1c5731c35.png)
Adding album cover (verbose):
![03-06](https://user-images.githubusercontent.com/18493608/161303058-d2c68d41-0df5-4692-9cbf-a43db4bb10e5.png)
![image](https://user-images.githubusercontent.com/18493608/161303977-e5f1db4a-0257-428e-8a85-36df51457300.png)
Replicating `albums` database:
![03-07](https://user-images.githubusercontent.com/18493608/161303061-9658cdf3-96b2-4332-b726-045272b510c6.png)

## Checkpoint 4
Running year query:
![04-01](https://user-images.githubusercontent.com/18493608/161303082-19203fd9-ac1a-4cc7-89f1-5df1585319df.png)
Finding all movies whose titles come after "L" in alphabetic order:
```
curl -X POST admin:admin@localhost:5984/hello-world/_find -d '{
  "selector": {
    "title": {
      "$gt": "L"
    }
  }
}' -H 'Content-Type: application/json'
```
Running above query:
![04-02](https://user-images.githubusercontent.com/18493608/161303135-5a41d319-6c2b-4eb5-93b9-01b8f2b54612.png)

Creating index based on title:
```
curl -X POST admin:admin@localhost:5984/hello-world/_index -d '{
   "index": {
      "fields": [
         "title"
      ]
   },
   "name": "title-json-index",
   "type": "json"
}' -H 'Content-Type: application/json'
```
Creating index with curl:
![04-03](https://user-images.githubusercontent.com/18493608/161303161-c95cffee-50c6-4722-af27-8b410aa41744.png)

Re-running query with index:
![04-04](https://user-images.githubusercontent.com/18493608/161303170-13bf9168-903a-454c-8c2e-e72f356c5230.png)
