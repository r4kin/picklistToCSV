# picklistToCSV

**READ ME**
Introduction -------------

The Apex Class, "YRK_Picklist_Metadata", intends to extract the dependent picklist values from a Custom Case Object (Service Area, Category, and Sub-Category), and output the combinations
of dependent fields to a csv file. Using Schema API in Apex, the "validFor" token of a given dependent picklist field can be revealed to show which controlling picklist values it is 
dependent on. A popular decoding method is used to convert the "validFor" Base64-encoded map to a string. The method "getPicklistValues(..)" returns a map of a controlling picklist object
to a list of dependent picklist objects. The method "getCsv(..)" merges , formats, and outputs the picklist labels of two dependent maps to a csv string. 

Requirements--------------   

- Access York Salesforce Production Sandbox
- Dependent multi-picklists on case object

Steps---------------------

1) Create Apex Class "YRK_Picklist_Metadata", copying code from "picklistToCsv_SCRIPT.txt"
2) Call the following methods: 
	Map<Object,List<String>> map1 = getPicklistValues(Case.Category__c);
	Map<Object,List<String>> map2 = getPicklistValues(Case.AY_SubCategory__c);
	String csvFile 		      = getCsv(map1,map2);
3) Export "csvFile" string
	
Explanation----------------
  
Uses Schema API to retrieve dependent picklist field object (ex. Category__c), and checks if picklist field has a controlling picklist (confirming the passed field is dependent). 
The code then iterates and adds all picklist entries of the controlling picklist field(Service_Area_c) to a list. Next the code iterates through the dependent picklist field's 
picklist entries and uses the JSON library to decode the "validFor" token and split the String to a list of characters. Using clever BitShift math, the code converts the list of 
characters to the 6 bits it represents (using the "base64map"). This 6 bit key, called "controlValue", gives the controlling field entry which is added to the picklist map. The code 
inserts the dependent picklist entry to the respective controlling picklist entry. 

Map<Object,List<String>> map1 = getPicklistValues(Case.Category__c);	      Returns: {A=(x,y,z)}
Map<Object,List<String>> map2 = getPicklistValues(Case.AY_SubCategory__c);    Returns: {X=(l,m,n), Y=(o,p), Z=(q,r,s,t)}  
