.. sectionauthor:: Roman Gainullov <roman.gaynullov@nextgis.com>

.. _ngw_permissions_system:

Permission system in NextGIS Web
================================

Mechanism of permission management is one of the key options of NextGIS Web.
Resources are the main entities in NextGIS Web and access permissions are managed at its level.

Mechanism of permission management of resources is similar to the principle of the file system access permissions.


.. _ngw_permissions_categories:

Permissions and its types (scopes)
---------------------------------

**Permission** - the ability to make various actions with resources. For example, ‘Read’ permission allows you to get man information about resources (e.g. name).
‘Update’ permission allows you to update this info.

For convenience permissions are grouped by **types (permission scope)**.
Listed above Read and Update permission examples are related to the main permission scope - ‘Resource’.
But there are some other types such as ‘Metadata’, ’Data structure’ or ‘Data’.


.. _ngw_permissions_control_list:

Access control list and the rules
----------------------------------

Permission management is carried out through **access control list (ACL)** changes which are linked to the resources.
In many ways, this is similar to Windows and Unix (POSIX ACL) OS permission management.
However NGW has much more features and actions on resources than filesystems.
Therefore there are more permissions and they are grouped into categories.

Access control list consists of the **rules** which have the following attributes:

* Action  - allow or deny
* Principal - user or group of users to which this rule applies
* Permission - permission or permission scope that is prohibited or allowed by the rule
* Apply to - scope of application: this resource, this and subresourcer or a particular type of resource to which this rule applies

Regardless of rule’s placement (at the beginning or end of the list) - first, the rules with the action “Allow” are applied, and then the rules “Deny”.
In other words - **‘Deny’ has a higher priority than ‘Allow’**. The position of the rule doesn't matter.

You can set a group of users as a principal (e.g. ‘editors’ group) - in the result this rule will be applied to such users who are in this group.
Also as a principal you can set a specific user. In this case the rule will be applied to this user.

In addition to the groups created by the administrator, the system has special system user groups:

* Administrators - group whose users have administrative rights
* Editors - a group whose users do not have access to the control panel, but can create and edit data

Adding users to these groups is a convenient way to quickly assign the necessary permissions throughout the system. These groups cannot be deleted.

Also, a specific user can be specified as a principal, in which case the rule will apply only to him.

Also NextGIS Web has multiple virtual system users to be used in access control lists:

* Authenticated - the rule will be applied to any authenticated user (logged in NGW)
* Guest - the rule will be applied to not  authenticated user
* Everyone - the rule will be applied to any user (authenticated or not)
* Owner - the rule will be applied to user who created a resource

If the **‘Apply to’** attribute is set to ‘This and subresources’, then the rule applies not only to the current resource but to child resources. This attribute also allows you to set rules limitations to specific subresource categories.


.. _ngw_permissions_relations:

Permissions dependencies
------------------------

Such a situation, if a user may change resource name but has no opportunity to read this name, is so weird and leads to inconsistent system behavior in general.
To avoid this problem NextGIS Web has permission dependencies.

For example, ‘Update’ permission depends on ‘Read’ permission.
Even if a user has a rule which allows him to ‘Update’ a resource but has not a rule to ‘Read’ it - then ‘Update’ permission will not take effect, ‘Update’ will be masked by ‘Read’.
In practice most permissions depend on ‘Read’ at least

Also there are dependencies between permissions of related resources. Let's consider the example of the file system hierarchy.
Suppose there is a hierarchy in the file system: **directory 1 > directory 2 > file**.

Here a user can be given the permission to read the file.
But if he does not have the opportunity to go to directory 1, and then to directory 2, he will not be able to read the file.

Similar behavior is implemented using the dependence of the “Read” permission of the child resource on the “Read” permission of the parent resource.

.. warning::   
   Thus, if you set the resource ‘Read’ permission then 
   it doesn't matter what permissions you assign to resources inside this folder, they won't take effect.


.. _ngw_effective_permissions:

Computing Effective Permissions
-------------------------------

Suppose the user is going to perform some operation on a resource, for example, read its name. 
When accessed, for example via API, NextGIS Web calculates **effective permissions** - the set of permissions that the user has in relation to a particular resource.
The computing is performed in the following sequence:

1. By default user does not have any permissions - the rule is **‘everything is deny except what is not explicitly allowed’**
2. Applied current resource and parent resources permissions that apply to ‘This and subresources’.
3. First the ‘Allow’ rules are applied -  permissions from them are added to the computed set of permissions.
4. After that, ‘Deny’ rules are applied - the permissions from them are subtracted from the calculated set of permissions.
5. Dependencies are checked, permissions with unsatisfied dependencies are marked as masked.

In the result you have an effective set of user permissions - permissions which are allowed, not denied and not masked by dependencies.
Based on this set NextGIS Web makes a decision about performing an action both in the API and in the web interface.


.. _ngw_first_entry:

Assigning permissions to users before their first sign in
----------------------------------------------------------

In NextGIS Web, users have the ability to sign in both as an internal NextGIS Web user and as a global account on my.nextgis.com.
In the second case, the administrator must add the global user account to `the team <https://docs.nextgis.com/docs_ngcom/source/create.html#team-management>`_ in its profile on my.nextgis.com or NextGIS ID on-premise server.

After sign in, a global user becomes a NextGIS Web user and is counted in the limit on their number.
However, by default, it does not have any permissions in NextGIS Web.

Therefore, we advise you to pre-set the permission type for a global user before its first auth.
There are two ways how you can do this:

* Preferred method: Assign permissions to some `user group <https://docs.nextgis.com/docs_ngweb/source/admin_tasks.html#ngw-create-group>`_ by checking the "New Users" flag. The user will be included in this group the first time they log in to NextGIS Web.
* Alternative way: assign resource permissions for the principal “Authenticated”.
