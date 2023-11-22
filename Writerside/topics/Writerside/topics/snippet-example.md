# snippet-example

<snippet id="generic_warning">

> This version support ended on January 14, 2022.
> Download the latest version.
>
>{style="note"}
> 
```plantuml
autonumber

participant mobile

box 'main_screen_meta' #LightBlue
participant main_screen_meta as msm
database Database as db
end box

participant Origination as orig
participant CardsAndAccounts as caa

mobile -> msm: getMainScreenMeta(x-plata-{mobile-headers})

alt #LightPink Unspecified error
mobile <-- msm: UNSPECIFIED
end

msm -> db: check current app vervion for app platform
msm <-- db: response

alt #LightPink {os}_unsupported_version >= x-plata-app-version
mobile <-- msm: VERSION_IS_OUTDATED
end

msm -> msm: get user data from x-plata-authorization-token and check claim

alt #LightGrey claim = Origination
msm -> orig: GetExtendedStatuses(GetStatusesRequest)
alt #LightPink Application in Progress Status
msm <-- orig: APPLICATION_IN_PROGRESS
end
alt #LightPink Pos Application with Claim = Origination
msm <-- orig: POS_IN_PROGRESS
end
msm <-- orig: Response
end

alt #LightGrey claim = Client
msm -> caa: GetAccountShort(GetAccountsShortRequest)

    msm <-- caa: Response
end


alt #LightPink Methods return invalid state
mobile <-- msm: INVALID_STATE
end

msm -> db: take Client configuration
msm <-- db: Respose[Configuration]

mobile <-- msm: GetMainScreenMetaResponse
```

</snippet>