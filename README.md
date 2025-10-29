Input_Payload------>>>>>>>>>>[Input_Payload]

<root>
   <BasicInfo>
      <seqId>8627_1998_04_22_IND</seqId>
      <empId>Abc8943</empId>
      <name>Kumar_SANU</name>
      <email>krsanu.gmail.com</email>
      <phone>+91 08498002008</phone>
      <startDate>2024-01-23T01:30:00.000</startDate>
      <address>901_ABC Road_Pokharai</address>
      <address2>Kolkata_WB_IND_700014</address2>
      <rating>2.98</rating>
   </BasicInfo>
   <BasicInfo>
      <seqId>9627_1988_04_22_USA</seqId>
      <empId>yvjc8943</empId>
      <name>HUSANU_SI</name>
      <email>husanu.gmail.com</email>
      <phone>+91 08002008</phone>
      <startDate>2022-06-23T01:30:00.000</startDate>
      <address>1_KK Road_DCBlock</address>
      <address2>DC_NZ_USA_867014</address2>
      <rating>3.6</rating>
   </BasicInfo>
</root>


AppendNode------>>>>>>>>>>[AppendNode]

import com.sap.gateway.ip.core.customdev.util.Message
import groovy.xml.XmlUtil
def Message AppendNode(Message message) {
    def body = message.getBody(String)
    def xmlBody = new XmlParser().parseText(body)
    xmlBody.BasicInfo.each{ it ->
        it.appendNode("PIN","748347")
    }
    message.setBody(XmlUtil.serialize(xmlBody))
    return message
}
