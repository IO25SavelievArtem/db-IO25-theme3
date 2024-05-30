# Проєктування бази даних

## Модель бізнес-об'єктів

@startuml

entity User #800000
entity User.id #808080
entity User.email #808080
entity User.password #808080
entity User.roleId #808080

User.id -up-* User 
User.email -up-* User 
User.password -up-* User 
User.roleId -up-* User

entity Role #800000
entity Role.id #808080
entity Role.name #808080
entity Role.description #808080

Role.id -up-* Role 
Role.name -up-* Role 
Role.description -up-* Role

entity PermHasRole #D2691E
entity PermHasRole.permisionId #808080
entity PermHasRole.roleName #808080

PermHasRole.permisionId --* PermHasRole 
PermHasRole.roleName --* PermHasRole

entity Permision #D2691E
entity Permision.id #808080
entity Permision.name #808080

Permision.id --* Permision 
Permision.name --* Permision

entity PubRequest #D2691E
entity PubRequest.name #808080
entity PubRequest.date #808080

PubRequest.name --* PubRequest 
PubRequest.date --* PubRequest

entity MediaData #D2691E
entity MediaData.id #808080
entity MediaData.name #808080
entity MediaData.fileType #808080

MediaData.id --* MediaData 
MediaData.name --* MediaData 
MediaData.fileType --* MediaData

entity Action #800000
entity Action.id #808080
entity Action.name #808080
entity Action.description #808080

Action.id --* Action 
Action.name --* Action 
Action.description --* Action

entity Guest #F0FFFF
entity RegisteredUser #F0FFFF
entity Admin#F0FFFF

entity AccountManage #F0FFFF
entity DataManage #F0FFFF
entity SearchManage #F0FFFF
entity SupportManage #F0FFFF

Guest ..> Role :instanceOf 
RegisteredUser ..> Role :instanceOf 
Admin ..> Role :instanceOf

AccountManage .up.> Action :instanceOf 
DataManage .up.> Action :instanceOf 
SearchManage .up.> Action :instanceOf 
SupportManage .up.> Action :instanceOf

Permision "1.1" -- "0." PermHasRole 
PermHasRole "0." -- "1.1" Role 
Role "1.1" -right- "0.*" User
MediaData "0." -- "1.1" PubRequest 
PubRequest "0." -- "1.1" Action
Action "0.*" -- "1.1" User

@enduml

## ER-модель

## Реляційна схема