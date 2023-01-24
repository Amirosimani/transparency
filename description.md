###### NODES #######
# PERSON
ٔnodes = {
			'Person': {'name' ,first_name', 'family_name', 'current_location', 'other_names', 'links'},
			'Org': {'org_name', 'location', 'links'}
		}


CREATE (ee:Person {name:'', first_name: 'سپیده', last_name: 'خندان دل', current_location: 'Eindhoven', links:'', nick_name:''})
CREATE (ee:Person {name:'', first_name: 'هوشنگ', last_name: 'خندان دل', current_location: 'Iran', links:'', nick_name:''})

CREATE (ee:Person {name:'', first_name: 'عبدالرضا', last_name: 'رحمانی', current_location: 'Iran', links:'', nick_name:''})


# ORG

CREATE (ee:Org {name: 'وزارت کشور', location: 'Iran', links:''})
CREATE (ee:Org {name: 'معاونت عمران و توسعه امور شهری و روستایی', location: 'Iran', links:''})
CREATE (ee:Org {name: 'بانک شهر', location: 'Iran', links:''})



========================================
###### RELATIONSHIPS #######
relationships = {
	"person-person": {"IS_RELATED_TO":{"type":{"فرزند"}}},
	"org-org": {"IS_PRANET_COMPNAY"},
	"per-org": {"WORKS_IN": {"title", "start_working", "end_working"},
				"STUDIES_IN": {"field", "start_studying", "end_studying"}
	}
}


#Person:Person#
{"IS_RELATED_TO"}

MATCH
  (a:Person),
  (b:Person)
WHERE a.first_name = 'سپیده' AND b.first_name = 'هوشنگ'
CREATE (a)-[r:IS_RELATED_TO {relationship_type: 'فرزند'}]->(b)
RETURN type(r)

MATCH
  (a:Person),
  (b:Person)
WHERE a.first_name = 'هوشنگ' AND b.first_name = 'عبدالرضا'
CREATE (a)-[r:IS_RELATED_TO {relationship_type: 'معاون'}]->(b)
RETURN type(r)

#Org:Org#

MATCH
  (a:Org),
  (b:Org)
WHERE a.name = 'وزارت کشور' AND b.name = 'معاونت عمران و توسعه امور شهری و روستایی'
CREATE (a)-[r:IS_PRANET_COMPNAY]->(b)
RETURN type(r)


# Per:Org
MATCH
(a:Person),
(b:Org)
WHERE a.first_name = 'هوشنگ' AND b.name='معاونت عمران و توسعه امور شهری و روستایی'
    CREATE (a)-[r:WORKS_IN {title: 'سرپرست', start_working:'', end_working:''}]->(b)
RETURN type(r)

MATCH
(a:Person),
(b:Org)
WHERE a.first_name = 'هوشنگ' AND b.name='بانک شهر'
    CREATE (a)-[r:WORKS_IN {title: 'عضو هیات مدیره', start_working:'', end_working:''}]->(b)
RETURN type(r)


MATCH
(a:Person),
(b:Org)
WHERE a.first_name = 'عبدالرضا' AND b.name='وزارت کشور'
    CREATE (a)-[r:WORKS_IN {title: 'وزیر', start_working:'15/09/2013', end_working:'25/08/2021'}]->(b)
RETURN type(r)

=========================================
### utils

LOAD CSV FROM "file:///Users/amir/Documents/projects/2022/shafaf_sazi/data"

# delete evreything
MATCH (n)
DETACH DELETE n

# delete by ID 7
MATCH (p:Person) where ID(p)=1
OPTIONAL MATCH (p)-[r]-() //drops p's relations
DELETE r,p

# to get the graph
MATCH p=()-->() RETURN p LIMIT 25
