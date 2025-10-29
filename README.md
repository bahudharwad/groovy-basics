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
