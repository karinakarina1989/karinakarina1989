import requests
from bs4 import BeautifulSoup
from datetime import datetime, timedelta

# List of URLs you want to check
urls = [
    #"https://www.fintrac-canafe.gc.ca/obligations/directives-eng",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/amlc-clrpc/atf-fat/Pages/osfi525_instr.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/amlc-clrpc/snc/Pages/osfi590_instr.aspx",
    #"https://www.fintrac-canafe.gc.ca/guidance-directives/client-clientele/1-eng",
    #"https://www.fintrac-canafe.gc.ca/guidance-directives/compliance-conformite/1-eng",
    #"https://www.fintrac-canafe.gc.ca/guidance-directives/compliance-conformite/rba/rba-eng",
    #"https://www.fintrac-canafe.gc.ca/guidance-directives/transaction-operation/1-eng",
    #"https://www.fintrac-canafe.gc.ca/guidance-directives/recordkeeping-document/1-eng",
    "https://laws-lois.justice.gc.ca/eng/acts/P-24.501/",
    "https://laws-lois.justice.gc.ca/eng/acts/C-45.2/",
    "https://laws-lois.justice.gc.ca/eng/acts/F-31.6/index.html",
    "https://laws.justice.gc.ca/eng/acts/J-2.3/index.html",
    "https://lois-laws.justice.gc.ca/eng/acts/S-14.5/",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2002-284/FullText.html",
    "https://laws-lois.justice.gc.ca/eng/acts/u-2/",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2002-184/",
    "https://lois-laws.justice.gc.ca/eng/regulations/SOR-2001-317/index.html",
    "https://laws-lois.justice.gc.ca/eng/acts/c-46/",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/CG_Guideline.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/in-ai/Pages/cbrsk.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/ifrs9.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/b1.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/b2.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/b10.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/B12.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/b7.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/b6-2020.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/e13.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/E17_final.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/e18.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/icaap_dti.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/e21.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/e23.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/e24.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/e6dti.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rai-eri/sp-ps/Pages/12-Internal_Audit.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rai-eri/sp-ps/Pages/06-Capital.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rai-eri/sp-ps/Pages/11-Risk_Management.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rai-eri/sp-ps/Pages/10-Senior_Management.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rai-eri/sp-ps/Pages/13-Compliance.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rai-eri/sp-ps/Pages/14-Financial_Analysis.aspx",
    "https://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/adv-prv/Pages/guidb2.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/CAR19_index.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/LR19.aspx",
    "http://www.osfi-bsif.gc.ca/Eng/fi-if/rg-ro/gdn-ort/gl-ld/Pages/LAR19_index.aspx",
    "https://payments.ca/systems-services/rules-documentation",
    "https://laws-lois.justice.gc.ca/eng/acts/b-1.01/index.html",
    "https://laws.justice.gc.ca/eng/acts/b-4/index.html",
    "https://laws-lois.justice.gc.ca/eng/acts/i-15/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2012-24/page-1.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-92-325/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-363/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2010-230/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2009-257/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2010-239/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-377/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2003-242/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-383/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-390/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2002-163/page-1.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-391/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-393/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2006-314/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-402/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2008-158/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-92-352/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-92-309/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-92-531/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-92-282/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-480/page-1.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2002-339/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2002-264/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-436/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-408/index.html",
    "https://laws.justice.gc.ca/eng/acts/C-3/",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2010-292/index.html",
    "https://www.cdic.ca/financial-community/for-cdic-members/data-and-system-requirements/",
    "https://laws.justice.gc.ca/eng/regulations/SOR-96-542/index.html",
    "https://www.cdic.ca/financial-community/for-cdic-members/brochures-signage-and-other-requirements/",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2019-312/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-93-516/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-99-120/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2003-346/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2017-1/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2016-283/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-475/page-1.html",
    "https://laws.justice.gc.ca/eng/acts/T-19.8/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-92-327/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2001-365/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2010-232/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2001-375/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-92-328/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2010-240/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2006-317/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2021-181/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-92-350/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-92-530/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-96-277/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-92-283/index.html",
    "https://laws.justice.gc.ca/eng/regulations/SOR-2002-266/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2001-55/index.html",
    "https://www.canada.ca/en/financial-consumer-agency/services/industry/supervision-framework.html",
    "https://laws.justice.gc.ca/eng/acts/C-34/index.html",
    "https://www.canada.ca/en/financial-consumer-agency/services/industry/commissioner-guidance/guidance-2.html",
    "https://www.canada.ca/en/financial-consumer-agency/services/industry/commissioner-guidance/guidance-3.html",
    "https://www.canada.ca/en/financial-consumer-agency/services/industry/commissioner-guidance/guidance-4.html",
    "https://www.canada.ca/en/financial-consumer-agency/services/industry/commissioner-guidance/guidance-12.html",
    "https://www.canada.ca/en/financial-consumer-agency/services/industry/forms-guides/mandatory-reporting-guide-frfi.html",
    "https://cba.ca/Assets/CBA/Documents/Files/Article%20Category/PDF/vol-seniors-en.pdf",
    "https://cba.ca/Assets/CBA/Documents/Files/Article%20Category/PDF/vol-poa-joint-account-en.pdf",
    "https://laws-lois.justice.gc.ca/eng/acts/E-1.6/index.html",
    "https://crtc.gc.ca/eng/archive/2014/2014-326.htm",
    "https://crtc.gc.ca/eng/archive/2018/2018-415.htm",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2013-221/page-1.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2012-36/index.html",
    "https://laws-lois.justice.gc.ca/eng/acts/L-2/",
    "https://laws-lois.justice.gc.ca/eng/regulations/C.R.C.,_c._986/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-86-304/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2015-164/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2020-130/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2020-260/index.html",
    "https://laws-lois.justice.gc.ca/eng/acts/e-5.401/page-1.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-96-470/index.html",
    "https://laws-lois.justice.gc.ca/eng/acts/H-6/",
    "https://laws-lois.justice.gc.ca/eng/acts/C-42/Index.html",
    "https://laws-lois.justice.gc.ca/eng/acts/t-13/",
    "https://laws-lois.justice.gc.ca/ENG/ACTS/P-8.6/index.html",
    "https://laws-lois.justice.gc.ca/eng/regulations/SOR-2018-64/index.html",
    "https://www.priv.gc.ca/en/privacy-topics/surveillance/police-and-public-safety/financial-transaction-reporting/faqs_pcmltfa_02/",
    "https://www.priv.gc.ca/en/privacy-topics/surveillance/police-and-public-safety/financial-transaction-reporting/faqs_pcmltfa_01/",
    "https://www.priv.gc.ca/en/privacy-topics/business-privacy/safeguards-and-breaches/privacy-breaches/respond-to-a-privacy-breach-at-your-business/c-t_201809_pb/",
    "https://www.priv.gc.ca/en/privacy-topics/privacy-laws-in-canada/the-personal-information-protection-and-electronic-documents-act-pipeda/r_o_p/gd_d1-d2_201703/",
    "https://www.priv.gc.ca/en/privacy-topics/identities/identification-and-authentication/auth_061013/",
    "https://www.priv.gc.ca/en/privacy-topics/privacy-laws-in-canada/the-personal-information-protection-and-electronic-documents-act-pipeda/p_principle/",
    "https://laws-lois.justice.gc.ca/eng/acts/I-3.3/",
    "https://laws-lois.justice.gc.ca/eng/regulations/C.R.C.,_c._945/index.html",
    "https://www.canada.ca/en/revenue-agency/services/tax/international-non-residents/enhanced-financial-account-information-reporting/reporting-sharing-financial-account-information-other-jurisdictions/guidance-on-common-reporting-standard-part-income-tax-act.html",
    "https://www.canada.ca/en/revenue-agency/services/tax/international-non-residents/enhanced-financial-account-information-reporting/reporting-sharing-financial-account-information-united-states/guidance-on-canada-s-enhanced-tax-information-exchange-agreement.html",

]

modified_within_last_30_days = []  # To store the URLs modified within the last 30 days
modified_dates_not_found = []  # To store the URLs for which modified date is not found

for url in urls:
    # Retrieve the HTML code of the website
    response = requests.get(url)
    html = response.text

    # Create a BeautifulSoup object to parse the HTML
    soup = BeautifulSoup(html, "html.parser")

    # Find the <dt> tag containing "Modified Date"
    dt_tags = soup.find_all("dt")

    for dt_tag in dt_tags:
        if dt_tag.text.strip() == "Modified Date:":
            # Find the <dd> tag following the <dt> tag
            dd_tag = dt_tag.find_next_sibling("dd")

            if dd_tag:
                # Get the date value from the <dd> tag
                modified_date = dd_tag.text.strip()

                # Convert the date string to a datetime object
                modified_datetime = datetime.strptime(modified_date, "%Y-%m-%d")
                print("Modified Date:", modified_datetime)

                # Calculate the current date and the date 30 days ago
                # CAN CHANGE THE DAYS BEING TESTED HERE
                current_datetime = datetime.now()
                hundred_days_ago = current_datetime - timedelta(days=30)

                # Compare the modified date with the date 30 days ago
                if modified_datetime >= hundred_days_ago:
                    print("\033[1;32m" + f"This website ({url}) has been modified within the last 30 days." + "\033[0m")
                    modified_within_last_30_days.append(url)
                else:
                    print("\033[1;31m" + f"This website ({url}) has not been modified within the last 30 days." + "\033[0m")
            break  # Break the loop once the modified date is found

    else:
        # If "Modified Date" not found, look for the alternative HTML structure
        meta_tags = soup.find_all("meta", attrs={"name": "dcterms.modified", "title": "W3CDTF"})

        if meta_tags:
            # Get the value of the "content" attribute
            modified_date = meta_tags[0].get("content")

            # Convert the date string to a datetime object
            modified_datetime = datetime.strptime(modified_date, "%Y-%m-%d")
            print("Modified Date:", modified_datetime)

            # CAN CHANGE THE DAYS BEING TESTED HERE
            current_datetime = datetime.now()
            hundred_days_ago = current_datetime - timedelta(days=30)

            if modified_datetime >= hundred_days_ago:
                print("\033[1;32m" + f"This website ({url}) has been modified within the last 30 days." + "\033[0m")
                modified_within_last_30_days.append(url)
            else:
                print("\033[1;31m" + f"This website ({url}) has not been modified within the last 30 days." + "\033[0m")

        else:
            print(f"Modified Date not found on the website: {url}")
            modified_dates_not_found.append(url)
    print()  # Print a blank line between each website analysis

print("\033[1;30m\033[1;43m\033[1mCOMPLETE: ALL THE WEBSITES HAVE NOW BEEN ANALYZED!\033[0m")
print("\n")

print("\x1B[4m" + "\033[1m" + "Websites Modified Within the Last 30 Days" + "\033[1m" + "\x1B[0m" + "\033[1;32m \u2713\033[0m" + ":") #ADDING GREEN CHECKMARK FOR VISUAL OUTPUT

if not modified_within_last_30_days:
    print("NONE.")
else:
    for url in modified_within_last_30_days:
        print(url)

if modified_dates_not_found:
    print("\n")
    print("\x1B[4m" + "\033[1m" + "Modified Dates Not Found for the Following Websites - Manual Check Advised" + "\033[1m" + "\x1B[0m" + "\033[1;31m \u2757\033[0m" + ":")
    for url in modified_dates_not_found:
        print(url)
    if not modified_dates_not_found:
      print("NONE.")
