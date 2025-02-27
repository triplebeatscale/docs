# Access management in {{ foundation-models-full-name }}

{% include [about-access-management](../../_includes/iam/about-access-management.md) %}

## Which resources you can assign a role for {#resources}

{% include [basic-resources](../../_includes/iam/basic-resources-for-access-control.md) %}

## Which roles exist in the service {#roles-list}

### Service roles {#service-roles}

#### ai.languageModels.user {#languageModels-user}

{% include [yandexgpt-user](../../_roles/ai/languageModels/user.md) %}

#### {{ roles-yaart-user }} {#imageGeneration-user}

{% include [yandexgpt-user](../../_roles/ai/imageGeneration/user.md) %}

#### ai.assistants.auditor {#assistants-auditor}

{% include [assistants-auditor](../../_roles/ai/assistants/auditor.md) %}

#### ai.assistants.viewer {#assistants-viewer}

{% include [assistants-viewer](../../_roles/ai/assistants/viewer.md) %}

#### ai.assistants.editor {#assistants-editor}

{% include [assistants-editor](../../_roles/ai/assistants/editor.md) %}

#### ai.assistants.admin {#assistants-admin}

{% include [assistants-admin](../../_roles/ai/assistants/admin.md) %}

#### ai.auditor {#ai-auditor}

{% include [ai-auditor](../../_roles/ai/auditor.md) %}

#### ai.viewer {#ai-viewer}

{% include [ai-viewer](../../_roles/ai/viewer.md) %}

#### ai.editor {#ai-editor}

{% include [ai-editor](../../_roles/ai/editor.md) %}

#### ai.admin {#ai-admin}

{% include [ai-admin](../../_roles/ai/admin.md) %}

### Primitive roles {#primitive-roles}

{% include [roles-primitive](../../_includes/roles-primitive.md) %}

{% include [primitive-roles-footnote](../../_includes/primitive-roles-footnote.md) %}

#### See also {#see-also}

* [How to assign a role](../../iam/operations/roles/grant.md)
* [How to revoke a role](../../iam/operations/roles/revoke.md)
* [Learn more about access management in {{ yandex-cloud }}](../../iam/concepts/access-control/index.md)
* [Learn more about inheriting roles](../../resource-manager/concepts/resources-hierarchy.md#access-rights-inheritance)