--- 
id: jira 
title: Jira 
--- 

# Jira

ToolJet can connect to a Jira workspace to do operations on jira issues, users, worklogs and boards. 

## Connection 
 
To establish a connection with the Jira data source, click on the `+Add new data source` button located on the query panel or navigate to the [Data Sources](https://docs.tooljet.com/docs/data-sources/overview) page from the ToolJet dashboard. 

For integrating Jira with ToolJet we will need the API token. The API token can be generated from your Jira workspace settings. Read the official Jira docs for [Basic auth for REST APIs](https://developer.atlassian.com/cloud/jira/platform/basic-auth-for-rest-apis/#get-an-api-token).

<div style={{textAlign: 'center'}}>

<img className="screenshot-full" src="/img/datasource-reference/jira/api.png" alt="jira api" />

</div>

## Querying Jira

Jira API provides support for:

- **[Issues](#issues)** 
- **[User](#user)** 
- **[Worklog](#worklog)** 
- **[Board](#board)** 

<img className="screenshot-full" src="/img/datasource-reference/jira/resource.png" alt="jira operations"/>

### Issues

On issues resource you can perform the following operations:

- **[Get issue](#1-get-issue)** 
- **[Create issue](#2-create-issue)** 
- **[Delete issue](#3-delete-issue)** 
- **[Assign issue](#4-assign-issue)** 
- **[Edit issue](#5-edit-issue)** 

<img className="screenshot-full" src="/img/datasource-reference/jira/issue.png" alt="jira issue" />

#### 1. Get issue

This operations retrieves an issue object using the issue key specified.

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.

##### Optional parameters

- **Params/Body**: You can add url parameters and body in this field. 
    
    **Query Parameters**:

    - fields: array
    - fieldsByKeys: boolean
    - expand: string
    - properties: array
    - updateHistory: boolean
    - failFast: boolean


#### 2. Create issue

This operation creates new issue.

##### Required parameters:

- **Params/Body**: You can add url parameters and body in this field.

    - fields: object. You must set a project key, issuetype and summary. 
    Example:

    ```
    fields:{
        project:{
            key:'KFTT'
        },
        issuetype:{
            name:'Story'
        },
        summary:'Summary'
    }
    ```

##### Optional parameters:

- **Params/Body**:
    - historyMetadata: HistoryMetadata
    - properties: array
    - transition: IssueTransition
    - update: object
    - Additional Properties: any

#### 3. Delete issue

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.

##### Optional parameters:

- **Delete Subtasks**: boolean

#### 4. Assign issue

Assigns an issue to a user.

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.
- **Account Id**: The request object with the user that the issue is assigned to.

#### 5. Edit issue

Edits an issue. Issue properties may be updated as part of the edit. Please note that issue transition will be ignored as it is not supported yet

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.

##### Optional parameters:

- **Params/Body**: You can add url parameters and body in this field.

    **Query parameters**:

    - notifyUsers: boolean
    - overrideScreenSecurit: boolean
    - overrideEditableFla: boolean
    - returnIssu: boolean
    - expan: string

     **Body**:
     - fields: object
     - historyMetadata: HistoryMetadata
     - properties: array
     - transition: IssueTransition
     - update: object
     - Additional Properties: any

### User

On user resource you can perform the following operations:

- **[Get user](#1-get-user)**
- **[Find users by query](#2-find-users-by-query)**
- **[Assignable users](#3-assignable-users)**

<img className="screenshot-full" src="/img/datasource-reference/jira/user.png" alt="jira user" />

#### 1. Get user

This operation retrieves a **User** object using the account ID.

##### Required parameters:

- **Account Id**: The account ID of the user, which uniquely identifies the user across all Atlassian products. For example, 5b10ac8d82e05b22cc7d4ef5.

##### Optional parameters:

- **expand**: Use expand to include additional information about users in the response.

#### 2. Find users by query

Finds users with a structured query and returns a paginated list of user details.

##### Required parameters:

- **Query**: string. The search query. Example:

`
is assignee of PROJ AND [propertyKey].entity.property.path is "property value"
`

##### Optional parameters:

- **Start At**: integer. The index of the first item to return in a page of results (page offset).
- **Max Results**: integer. The maximum number of items to return per page.

#### 3. Assignable users

Returns a list of users that can be assigned to an issue. Use this operation to find the list of users who can be assigned to:a new issue, by providing the projectKeyOrId, an updated issue, to an issue during a transition (workflow action).

##### Parameters:

- **Query**: string
- **Session Id**: string
- **Account Id**: string
- **Project Key**: string
- **Issue Key**: string
- **Start At**: integer
- **Max Results**: integer
- **Action Descriptor Id**: integer
- **Recommend**: boolean

### Worklog

The following operations can be performed on the worklog resource:

- **[Get issue worklogs](#1-get-issue-worklogs)**
- **[Add worklog](#2-add-worklog)**
- **[Delete worklog](#2-delete-worklog)**

<img className="screenshot-full" src="/img/datasource-reference/jira/worklog.png" alt="jira worklog" />

#### 1. Get issue worklogs

Returns worklogs for an issue (ordered by created time), starting from the oldest worklog or from the worklog started on or after a date and time.

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.

##### Optional parameters:

- **Start At**: integer
- **Max Results**: integer
- **Started After**: integer
- **Started Before**: integer
- **expand**: string

#### 2. Add worklog

Adds a worklog to an issue.

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.

##### Optional Parameters:


- **Params/Body**: You can add url parameters and body in this field.

    **Query parameters**:
    - notifyUsers: boolean
    - adjustEstimate: string
    - newEstimate: string
    - reduceBy: string
    - expand: string
    - overrideEditableFlag: boolean

    **Body**:
    - comment: any
    - properties: arr
    - started: string
    - timeSpent: string
    - timeSpentSeconds: integer
    - visibility: Visibility
    - Additional Properties: any

#### 3. Delete worklog

Deletes a worklog from an issue.

##### Required parameters:

- **Issue Key**: You'll find the issue Id or Key in the jira project.
- **Worklog ID**: The ID of the worklog.

##### Optional Parameters:

- **Params/Body**: You can add url parameters and body in this field.

    **Query parameters**:
    - notifyUsers: boolean
    - adjustEstimate: string
    - newEstimate: string
    - increaseBy: string
    - overrideEditableFlag: boolean

### Board

On board resource you can perform the following operations:

- **[Get issues for backlog](#1-get-issues-for-backlog)**
- **[Get all boards](#2-get-all-boards)**
- **[Get issues for board](#3-get-issues-for-board)**

<img className="screenshot-full" src="/img/datasource-reference/jira/board.png" alt="jira board" />

#### 1. Get issues for backlog

Returns all issues from the board's backlog, for the given board ID.

:::tip

Before querying Backlog, you must turn it on in your Jira project. Go to `+Add view` section in your project, find and activate `Backlog`

<img className="screenshot-full" src="/img/datasource-reference/jira/turn-backlog.png" alt="jira backlog"/>

:::

##### Required parameters:

- **Board Id**: The ID of the board that has the backlog containing the requested issues.

##### Optional parameters:

- **Start At**: integer
- **Max Results**: integer
- **expand**: Use expand to include additional information about users in the response.
- **Params/Body**: You can add url parameters and body in this field.

    **Query parameters**:
    - jql: string
    - validateQuery: boolean
    - fields: array

#### 2. Get all boards

Returns all boards. This only includes boards that the user has permission to view.

##### Optional parameters:

- **Project Key**: string
- **Start At**: integer
- **Max Results**: integer
- **expand**: Use expand to include additional information about users in the response.
- **Params/Body**: You can add url parameters and body in this field.

    **Query parameters**:
    - type: object
    - name: string
    - projectKeyOrId: string
    - accountIdLocation: string
    - projectLocation: string
    - includePrivate: boolean
    - negateLocationFiltering: boolean
    - orderBy: string
    - expand: string
    - projectTypeLocation: array
    - filterId: integer

#### 3. Get issues for board

Returns all issues from a board, for a given board ID. This only includes issues that the user has permission to view.

##### Required parameters:

- **Board Id**: The ID of the board that has the backlog containing the requested issues.

##### Optional parameters:

- **Start At**: integer
- **Max Results**: integer
- **expand**: Use expand to include additional information about users in the response.
- **Params/Body**: You can add url parameters and body in this field.

    **Query parameters**:
    - jql: string
    - validateQuery: boolean
    - fields: array

[Read more about jira API](https://developer.atlassian.com/cloud/jira/platform/)