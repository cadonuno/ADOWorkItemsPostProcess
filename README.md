# ADO Work Items Post Process:

Adds additional capabilities to the Veracode Flaw Importer, allowing for the linkage of issues (to an epic for instance), and default user assignment.

# Usage:
After running a regular flaw importer, call this:  

      # requires AZURE_API_CREDENTIALS environment variable (secret)  
      - template: post-process-work-items.yml 
        parameters:
          organization: '{{ORGANIZATION_NAME}}'
          project_name: '{{PROJECT_NAME}}'
          azure_base_directory: '{{azure_base_url}}'
          work_item_id_to_link: '{{WORK_ITEM_ID}}'
          user_to_assign: '{{USER_DESCRIPTOR}}'  #user descriptor
          tag_value_to_filter_issues: '{{TAG_TO_FILTER}}'

# Parameter descriptions:
- organization and project_name: both are part of the project URL on Azure
  - i.e.: For this scenario, https://dev.azure.com/{ORGANIZATION_NAME}/{PROJECT_NAME}
    - organization: ORGANIZATION_NAME
    - project_name: PROJECT_NAME
- azure_base_directory: the base URL of your Azure environment. Usually https://dev.azure.com, but can be different if self-hosted
  - i.e. :https://dev.azure.com
- work_item_id_to_link: the ID of the work item you want to link the new findings to.
- user_to_assign: the user descriptor of the user to assign.
  - You can follow these instructions to obtain it: https://learn.microsoft.com/en-us/rest/api/azure/devops/graph/users/list
  - You can also assign a single ticket to the user you want, and call this API: https://dev.azure.com/{ORGANIZATION_NAME}/{PROJECT_NAME}/_apis/wit/workItems/{WORK_ITEM_ID}
    - The descriptor will be under fields->System.AssignedTo->descriptor
- tag_value_to_filter_issues: a unique tag that will be present in all the tickets you want to update.
  - If selecting the 'Add Scan Name as a Tag' in the Flaw Importer task, you would have the scan name (version) here.
