import com.sap.gateway.ip.core.customdev.util.Message;
import java.util.HashMap;
import groovy.xml.*
import groovy.xml.StreamingMarkupBuilder;
import groovy.xml.XmlUtil;
import java.lang.String;
import java.text.DateFormat;
import java.text.DecimalFormat;
import java.time.LocalDate;
import java.time.*
import java.time.Period;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import groovy.json.JsonSlurper;
import com.sap.it.api.securestore.SecureStoreService;
import com.sap.it.api.securestore.UserCredential;
import com.sap.it.api.ITApiFactory;
import javax.xml.xpath.*;
import javax.xml.parsers.DocumentBuilderFactory;
import java.time.format.DateTimeFormatter;
    
class PersonInfoData {
    String person_id_external = "";
    String last_modified_on = "";
    String first_name = "";
    String last_name = "";
    String salutation = "";
    String gender = "";
    String employment_id = "";
    String start_date = "";
    String business_unit = "";
    String company = "";
    String job_title = "";
    String manager_id = "";
    String benefits_rate = "";
    String is_eligible_for_benefits = "";
}

def Message Init(Message message) {
    HashMap<String, PersonInfoData> mapPersonInfoData = new HashMap<String, HashMap>();
    HashMap<String, String> mapSalutationlookUp= new HashMap<String, String>();
    HashMap<String, String> mapGenderlookUp = new HashMap<String, String>();
    HashMap<String, String> mapBusinessUnitlookUp = new HashMap<String, String>();
    HashMap<String, String> mapCompanylookUp = new HashMap<String, String>();
    
    message.setProperty("mapPersonInfoData", mapPersonInfoData);
    message.setProperty("mapSalutationlookUp", mapSalutationlookUp);
    message.setProperty("mapGenderlookUp", mapGenderlookUp);
    message.setProperty("mapBusinessUnitlookUp", mapBusinessUnitlookUp);
    message.setProperty("mapCompanylookUp", mapCompanylookUp);
    return message;
}

def Message CachePickList(Message message) {
    def body = message.getBody(java.lang.String)
    def xmlBody = new XmlSlurper().parseText(body);
    
    def properties = message.getProperties();
    HashMap<String, String> mapSalutationlookUp = properties.get("mapSalutationlookUp");
    HashMap<String, String> mapGenderlookUp = properties.get("mapGenderlookUp");
    
    xmlBody.PickListValueV2.each { PickListValueV2 ->
        if(PickListValueV2.PickListV2_id.text()=='salutation')
        {
            mapSalutationlookUp.put(PickListValueV2.nonUniqueExternalCode.text(), PickListValueV2.label_en_US.text())
        }  
        if(PickListValueV2.PickListV2_id.text()=='Gender')
        {
            mapGenderlookUp.put(PickListValueV2.externalCode.text(), PickListValueV2.label_en_US.text())
        }
    }
    return message;
}

def Message CacheEmpJob(Message message) {
    def body = message.getBody(java.lang.String)
    def xmlBody = new XmlSlurper().parseText(body);
    
    def properties = message.getProperties();
    HashMap<String, String> mapBusinessUnitlookUp = properties.get("mapBusinessUnitlookUp");
    HashMap<String, String> mapCompanylookUp = properties.get("mapCompanylookUp");
    
    xmlBody.EmpJob.businessUnitNav.FOBusinessUnit.each { businessUnit ->
        mapBusinessUnitlookUp.put(businessUnit.externalCode.text(), businessUnit.name_en_US.text());
    }
    
    xmlBody.EmpJob.companyNav.FOCompany.each { company ->
        mapCompanylookUp.put(company.externalCode.text(), company.name_en_US.text());
    }
    
    return message;
}

def Message CacheDataFromSF(Message message) {
    def body = message.getBody(java.lang.String)
    def xmlBody = new XmlSlurper().parseText(body);
    // def xml = new XmlSlurper();
    // def data = xml.parseText(body);
    
    for (i = 0; i < xmlBody.CompoundEmployee.size() && i < 10; i++) {
        def newbody = xmlBody.CompoundEmployee[i]
    
    def properties = message.getProperties();
    HashMap<String, PersonInfoData> mapPersonInfoData = properties.get("mapPersonInfoData");
    HashMap<String, String> mapSalutationlookUp = properties.get("mapSalutationlookUp");
    HashMap<String, String> mapGenderlookUp = properties.get("mapGenderlookUp");
    HashMap<String, String> mapBusinessUnitlookUp = properties.get("mapBusinessUnitlookUp");
    HashMap<String, String> mapCompanylookUp = properties.get("mapCompanylookUp");
    
    def formatDate = { inputDate ->
        Date date = Date.parse("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'", inputDate)
        SimpleDateFormat sdf = new SimpleDateFormat("dd-MMM-yyyy");
        return sdf.format(date);
    }
    
    def formattedDate = { ipDate ->
        Date sDate = Date.parse("yyyy-MM-dd", ipDate)
        SimpleDateFormat sdf = new SimpleDateFormat("dd-MMM-yyyy");
        return sdf.format(sDate);
    }
    
    // for (i = 0; i < data.CompoundEmployee.size() && i < 10; i++) {
    //     def xmlBody = data.CompoundEmployee[i]
    
    newbody.person.each { personal ->
        PersonInfoData personInfo = new PersonInfoData();
    
        personInfo.person_id_external = personal.person_id_external.text();
        personInfo.last_modified_on = personal.last_modified_on.text();
        
        personal.personal_information.each { perInfo ->
                    
            personInfo.salutation = mapSalutationlookUp.get(perInfo.salutation.text());
            personInfo.first_name = perInfo.first_name.text();
            personInfo.last_name = perInfo.last_name.text();
            personInfo.gender = mapGenderlookUp.get(perInfo.gender.text());
        }
    
        personal.employment_information.each { empInfo ->
            personInfo.employment_id = empInfo.employment_id.text();
            personInfo.start_date = empInfo.start_date.text();
                
            empInfo.job_information.each {  jobInfo ->
                personInfo.business_unit = mapBusinessUnitlookUp.get(jobInfo.business_unit.text());
                personInfo.company = mapCompanylookUp.get(jobInfo.company.text());
                personInfo.job_title = jobInfo.job_title.text();
                personInfo.manager_id = jobInfo.manager_id.text();
            }
    
            empInfo.compensation_information.each { compInfo ->
                personInfo.benefits_rate = compInfo.benefits_rate.text();
                personInfo.is_eligible_for_benefits = compInfo.is_eligible_for_benefits.text();
            }
        }
        String keyP = personal.person_id_external.text();
        mapPersonInfoData.put(keyP, personInfo)
    }
            
    String separatorChar = "|";
    StringBuilder sbItems = new StringBuilder();
    
    AppendValue(sbItems, "person_id_external"   , separatorChar, false, false);
    AppendValue(sbItems, "last_modified_on" , separatorChar, false, false);
    AppendValue(sbItems, "salutation"   , separatorChar, false, false);
    AppendValue(sbItems, "first_name"   , separatorChar, false, false);
    AppendValue(sbItems, "last_name"   , separatorChar, false, false);
    AppendValue(sbItems, "gender"   , separatorChar, false, false);
    AppendValue(sbItems, "employment_id"   , separatorChar, false, false);
    AppendValue(sbItems, "start_date"   , separatorChar, false, false);
    AppendValue(sbItems, "business_unit"   , separatorChar, false, false);
    AppendValue(sbItems, "company"   , separatorChar, false, false);
    AppendValue(sbItems, "job_title"   , separatorChar, false, false);
    AppendValue(sbItems, "manager_id"   , separatorChar, false, false);
    AppendValue(sbItems, "benefits_rate"   , separatorChar, false, false);
    AppendValue(sbItems, "is_eligible_for_benefits"   , separatorChar, false, true);
    
    mapPersonInfoData.each {keyP, personInfo ->
        AppendValue(sbItems, personInfo.person_id_external  , separatorChar, false, false);
        // AppendValue(sbItems, personInfo.last_modified_on    , separatorChar, false, false);
        AppendValue(sbItems, formatDate(personInfo.last_modified_on)    , separatorChar, false, false);
        AppendValue(sbItems, personInfo.salutation  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.first_name  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.last_name  , separatorChar, false, false);  
        AppendValue(sbItems, personInfo.gender  , separatorChar, false, false); 
        AppendValue(sbItems, personInfo.employment_id  , separatorChar, false, false);
        // AppendValue(sbItems, personInfo.start_date  , separatorChar, false, false);
        AppendValue(sbItems, formattedDate(personInfo.start_date)    , separatorChar, false, false);
        AppendValue(sbItems, personInfo.business_unit  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.company  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.job_title  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.manager_id  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.benefits_rate  , separatorChar, false, false);
        AppendValue(sbItems, personInfo.is_eligible_for_benefits  , separatorChar, false, true);
    }
        
    message.setBody(sbItems);
    }
    return message;
}

def void AppendValue(StringBuilder sbItems, String stringToAppend, String separatorChar, boolean wrapInQuotes, boolean endOfLine) {
    if (endOfLine) {
        separatorChar = "\n";
    }
    if (!wrapInQuotes) {
        sbItems.append("${stringToAppend}").append(separatorChar)
    }
    else
    {
        sbItems.append("\"${stringToAppend}\"").append(separatorChar)
    }
}
