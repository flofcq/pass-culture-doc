### Contremarques [/booking/]

Ceci décrit l'API utilisable par les partenaires techniques de Pass Culture qui souhaitent valider des contremarques.

Les partenaires s'identifient auprès de l'API Pass Culture à l'aide d'un jeton disponible dans la section "structure" du portail pro.

**Statut actuel : ébauche, pour discussions**

##### Vérifier une contremarque [GET /bookings/token/<token>]

Vérifie la validité d'une contremarque et récupère les informations de la transaction associée (uniquement si la contremarque est associée à une offre du partenaire identifié sur l'API).

+ Parameters

  + token (string) - le code de réservation
  + email (optional, string) - obligatoire si le partenaire n'est pas identifié, correspond à l'email de la personne qui tente d'utiliser la contremarque pour obtenir une offre
  + offerId (optional, string) - identifiant de l'offre supposée correspondre à la réservation

+ Request (application/json)

    + Body
    
            [
              {
                 "token": "AXB32J", 
                 "email": "test@test.com",
                 "offerId": "ABDE"
              }
            ]

+ Response 200 (application/json) dans le cas où le partenaire est identifié sur l'API.

    + Body

            [
              {
                "bookingId": "ABC", 
                "email": "test@test.com",
                "userName": "John Doe",
                "offerName": "Offre Test", 
                "date": "2018-09-18T10:50:34.076546Z"
                "isUsed": False,
                "venueDepartementCode": "93"
              }
            ]
            
+ Response 204 (application/json) dans le cas où le partenaire n'est pas identifié sur l'API.

+ Response 400 (application/json)

    + Body

            [
              {
                 "global": "Ce coupon n'a pas été trouvé"
              }
            ]

+ Response 400 (application/json) dans le cas où le partenaire n'est pas identifié

    + Body

            [
              {
                 "email": "Vous devez préciser l\'email de l\'utilisateur quand vous n\'êtes pas connecté(e)"
              }
            ]

##### Valider une contremarque [PATCH /bookings/token/<token>]

Valide une contremarque (et la transaction associée). Le partenaire doit obligatoirement être identifié.

+ Parameters

  + email (optional, string) - correspond à l'email de la personne qui tente d'utiliser la contremarque pour obtenir une offre
  + token (string) - le code de réservation
  + offerId (optional, string) - identifiant de l'offre supposée correspondre à la réservation

+ Request (application/json)

    + Body
       { 
         "token": "AXB32J", 
         "email": "test@test.com",
         "offerId": "ABDE"
       }

+ Response 204 (application/json)

+ Response 400 (application/json)

    + Body

            [
              {
                 "global": ""Cette structure n'est pas enregistrée chez cet utilisateur.""
              }
            ]

+ Response 404 (application/json)

+ Response 410 (application/json)

    + Body

            [
              {
                 "booking": "Cette réservation a été annulée"
              }
            ]


+ Response 410 (application/json)

    + Body

            [
              {
                 "booking": "Cette réservation a déjà été validée"
              }
            ]
