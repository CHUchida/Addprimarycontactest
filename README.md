@isTest
public class AddPrimaryContactTest {
    
    static testmethod void testQueueable() {
        List <Account> testAccounts = New List<Account()>;
        for(integer i=0;i<50;i++){
            testAccounts.add(new Account(Name='Account '+i,
                                         BillingState = 'CA'));
        }
        for(integer j=0;j<50;j++){
            testAccounts.add(new Account(Name='Account '+j,
                                         BillingState = 'NY'));
        }
        insert testAccounts;
        
        Contact testContact = new Contact(FirstName='John', LastName = 'Doe');
		insert testContact;       
                                          
AddPrimaryContact addit  = new AddPrimaryContact(testContact, 'CA');
// startTest/stopTest block to force async processes to run
Test.startTest();        
System.enqueueJob(addit);
Test.stopTest();        
// Validate the job ran. Check if record have correct parentId now
System.assertEquals(50, select count() from Contact where accountId in (Select Id from Account where BillingState ='CA')]);
  }
}
                                          
