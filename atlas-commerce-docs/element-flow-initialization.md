{
  "eCommerce": {
    "hostedPaymentFields": {
      "enabled": true,
      "controls": [
        {
          "cardType": "credit",
          "container": "#credit-container",
          "fieldOptions": {
            "fieldGroupingType": "individual",
            "fields": [
              {
                "fieldType": "Pan",
                "validationType": "required",
                "maskingType": "maskOnBlur",
                "maskingCharacter": "dot",
                "placeHolderType": "message",
                "controlLabelPosition": "outlineLegend",
                "validationMessageDisplayType": "feedback"
              },
              {
                "fieldType": "ExpDate",
                "validationType": "required",
                "placeHolderType": "default",
                "controlLabelPosition": "outlineLegend",
                "validationMessageDisplayType": "feedback"
              },
              {
                "fieldType": "SecurityCode",
                "validationType": "required",
                "placeHolderType": "cvc",
                "controlLabelPosition": "outlineLegend",
                "validationMessageDisplayType": "feedback"
              }
            ]
          }
        }
      ]
    }
  },
  "credentials": {
    "storeId": "STR_99203",
    "terminalId": "TRM_88102"
  }
}
