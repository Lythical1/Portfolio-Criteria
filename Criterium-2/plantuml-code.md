Ter backup van de fotos

[Link](https://editor.plantuml.com/uml/RPDDJyCm38Rl_XNMBfmcOD_R0MqQ740W9YQuUwtRXJL9bQHC4-A_atOjj6jloUFNJlp6MLf7ncKo0NiGAYL34YbmscpTAM6a8rij6HAE73gDHiHxXw4m73YX5S1Y5KgX5aWMAL_1ua6cYJwnAHYN6rWywHrkcGzbu5FlRB43o6kHYlrfFI_QfjhX9Y4NQIDx-0s8cUM0h0-_SIoiOtFzh6EXUdcTz_LjNV52YcB6ZT6HIBXK3EgAZROEyykyHX4RMqg6TScMoGJxN5I5HurmjfF2uIfD4n5GRaCb6zTbQyFa6F_xlhZLf54ps7EOUGiUbU_lI_2ngZdjQ6-jPKfASAMPrJKVFXdloUvtcanOAkQDmqxUH8d5ota_JT53vUOD_01SeRT9vFWy31RzAXIvOwmSMX6oXUAziP-b_RjpOXFCB_OpW6eQ_fFQQD16mpQQjj6iZUO0VtE6_L0VLNbg5nrqqb4d7NVIIIUTmv9Lw7_OBm00)

@startuml
skinparam actorStyle awesome
left to right direction

:Admin: as admin
:Employer: as employer
:Job Seeker: as jobseeker

rectangle "JobSpot Platform" {
    usecase "Register/Login" as UC1
    usecase "Manage Profile" as UC2
    usecase "Post Job Offers" as UC3
    usecase "Search Jobs" as UC4
    usecase "Submit Applications" as UC5
    usecase "Create Job Seeker Profile" as UC6
    usecase "Schedule Interviews" as UC7
    usecase "Manage Companies" as UC8
    usecase "View Dashboard" as UC9
    usecase "System Administration" as UC10
}

admin -up-> UC10
admin -up-> UC9
admin -up-> UC8

employer -up-> UC1
employer -up-> UC2
employer -up-> UC3
employer -up-> UC7
employer -up-> UC8
employer -up-> UC9

jobseeker -down-> UC1
jobseeker -down-> UC2
jobseeker -down-> UC4
jobseeker -down-> UC5
jobseeker -down-> UC6
jobseeker -down-> UC9
@enduml