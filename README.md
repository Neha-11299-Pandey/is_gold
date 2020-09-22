# is_gold
When Amount field on Opportunity  is greater than $20k, mark is_gold to TRUE.
trigger UpdateAccountField on Opportunity (after insert,after update) {
    List<Account> lstAccounts = new  List<Account>();
    Set<Id> setAccountIds = new Set<Id>();
    Map<Id,Opportunity> mapAccIdOpp = new Map<id, Opportunity>();
    
    if(Trigger.isInsert || Trigger.isUpdate && Trigger.isAfter){
    for(Opportunity obj : Trigger.new){
        setAccountIds.add(obj.AccountId);
      }
    }
    
    for(Opportunity obj : Trigger.new){
        mapAccIdOpp.put(obj.AccountId,obj);
        
    }
    
    for(Account obj : [SELECT Id,is_gold__c FROM Account WHERE Id IN: setAccountIds]){
        if(mapAccIdOpp.get(obj.Id).Amount > 20000){
            obj.is_gold__c = TRUE;
            lstAccounts.add(obj);
        }
        
        else
            obj.is_gold__c = FALSE;
             lstAccounts.add(obj);
        
      }
   }
