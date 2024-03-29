## befehle für neo4j in Bezug auf Human Connection Daten

Wenn Human Connection bei euch local läuft dann erreicht ihr unter *http://localhost:7474/browser/* die neo4j Datenbank

![ezgif com-crop (1)](https://user-images.githubusercontent.com/1324583/67863204-bf18a000-fb23-11e9-9df6-15a918b63107.gif)



___
# 1. Befehle - allgemein
  1.1 Alle Knoten und Kanten (Alle Einträge und ihre Verbindungen) 
  
      1.1.1 `MATCH (n) RETURN n`
      1.1.2  MATCH (n) RETURN n LIMIT 25 
![FireShot Capture 128 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/67861482-98a53580-fb20-11e9-8c0a-8bb3aa0b028a.png)



1.2 Alle Badge Knoten 

      1.2.1 MATCH (n:Badge) RETURN n 
      1.2.2 MATCH (n:Badge) RETURN n LIMIT 25
    


1.3 Alle Category Knoten 

      1.3.1 MATCH (n:Category) RETURN n
      1.3.2 MATCH (n:Category) RETURN n LIMIT 25 
    

![FireShot Capture 131 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/67862172-ea9a8b00-fb21-11e9-97a8-9d948f45610d.png)


1.4 Alle Beiträge
     
     1.4.1 MATCH (n:Post) RETURN n
     1.4.2 MATCH (n:Post) RETURN n LIMIT 25
     
   Alle Beiträge eines Users
   
     1.4.3 MATCH (p:User{name: "Peter Lustig"})-[:WROTE]-(t:Post) return  p, t
     
     ### anstelle des Namens eines Users kann hier natürlich die UserId Verwendet werden ;) 
     aus {name: "Peter Lustig"} wird dann einfach {id: "USERID"}
     
![ezgif com-video-to-gif](https://user-images.githubusercontent.com/1324583/67864543-0ef86680-fb26-11e9-8fbe-98ca79d47c31.gif)

  Alle Shouted Beiträge eines Users
   
    1.4.4 MATCH (p:User{name: "Peter Lustig"})-[:SHOUTED]-(t:Post) return  p, t
     
 ![FireShot Capture 132 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/67864989-d3aa6780-fb26-11e9-8e33-5b4e3a39085a.png)

Alle Shouted Beiträge eines Users und ihrer Verfasser
  
      1.4.5. MATCH (p:User{name: "Peter Lustig"})-[:SHOUTED]-(t:Post)-[:WROTE]-(u:User) return p, t,u

![FireShot Capture 135 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/67870902-d9587b00-fb2f-11e9-9012-685ef019a027.png)


Alle verfassten Kommentare eines Users

      1.4.6. MATCH (p:User{name: "Peter Lustig"})-[:WROTE]-(t:Comment) return p, t
     
     
Alle verfassten Kommentare eines Users und die Beiträge dafür

       1.4.7. MATCH (p:User{name: "Peter Lustig"})-[:WROTE]-(t:Comment)-[:COMMENTS]-(x:Post) return p, t, x
       
![FireShot Capture 133 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/67866115-b70f2f00-fb28-11e9-97f3-f89878f64894.png)


Alle verfassten Kommentare eines Users, die dazugehörigen Beiträge und der Ersteller des Beitrags

       1.4.8. MATCH (p:User{name: "Peter Lustig"})-[:WROTE]-(t:Comment)-[:COMMENTS]-(x:Post)-[:WROTE]-(z:User) return p,t,x,z

![FireShot Capture 134 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/67866114-b70f2f00-fb28-11e9-96b5-6653d31f7e41.png)


Alle Freunde eines Users

      MATCH (p:User{name: "Peter Lustig"})-[:FRIENDS]-(u:User) return u
   
Alle User denen Peter Lustig folgt und die ihm folgen
   
    MATCH (p:User{name: "Peter Lustig"})-[:FOLLOWS]-(u:User) return p,u
       
Alle User denen Peter Lustig folgt

      MATCH (p:User{name: "Peter Lustig"})-[:FOLLOWS]->(u:User) return u
      
Alle User welche Peter Lustig folgen

      MATCH (p:User{name: "Peter Lustig"})<-[:FOLLOWS]-(u:User) return u


Alle User die einem User folgen

    MATCH (p:User)-[:FOLLOWS]-(u:User) return p,u



# Beiträge

![FireShot Capture 163 - bolt___localhost_7687 - Neo4j Browser - localhost](https://user-images.githubusercontent.com/1324583/68188734-1c8c7100-ffaa-11e9-98ab-dbcf5786cc0a.png)


Alle beiträge in alphabetischer Reinfolge [Aa - Zz]

      MATCH (n:Post) RETURN n ORDER BY toLower(n.title) ASC
      

Alle beiträge in alphabetischer Reinfolge [Zz - Aa]

      MATCH (n:Post) RETURN n ORDER BY toLower(n.title) DESC
      
      
Geordnet nach neuste Beiträge zuerst [Datum absteigend]

      MATCH (n:Post) RETURN n ORDER BY n.createdAt DESC
      
      
Geordnet nach älteste Beiträge zuerst  [Datum aufsteigend]

      MATCH (n:Post) RETURN n ORDER BY n.createdAt ASC

Alle Beiträge mit ihren Kommentaren 

      MATCH (p:Post)-[:COMMENTS]-(c:Comment) return p,c Order By c.createdAt
      
Geordnet nach neuster Kommentierter Beitrag [Datum absteigend]

       MATCH (p:Post)-[:COMMENTS]-(c:Comment) return p,c Order By c.createdAt DESC 
       
Geordnet nach ältester Kommentierter Beitrag [Datum aufsteigend]

       MATCH (p:Post)-[:COMMENTS]-(c:Comment) return p,c Order By c.createdAt ASC 
