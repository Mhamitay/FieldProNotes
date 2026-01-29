Validation for save and update
Here is an example for a domain validation

``` C#
public static List<string> Validate(Models.Asset model, Validate.Action action)
{
    var errors = new List<string>();

    // Common for update actions
    if (action == Validate.Action.Update || action == Validate.Action.Update2)
    {
        if (model.ID <= 0)
            errors.Add("ID is required for update.");
    }

    // Basic validation for insert and update
    if (action == Validate.Action.Insert || action == Validate.Action.Update)
    {
        if (model.AssetClassificationID <= 0)
            errors.Add("Asset Classification is required.");

        if (model.AssetTypeID <= 0)
            errors.Add("Asset Type is required.");

        if (model.AssetSubTypeID <= 0)
            errors.Add("Asset Sub Type is required.");

        if (model.AssetSubTypeDescriptionID <= 0)
            errors.Add("Description is required.");

        if (string.IsNullOrWhiteSpace(model.SerialNumber))
            errors.Add("Serial Number is required.");

        if (model.Length <= 0)
            errors.Add("Length must be greater than zero.");
    }

    // Extended fields for Update2
    if (action == Validate.Action.Update2)
    {
        if (model.PurchasePrice < 0)
            errors.Add("Purchase Price must be a positive number.");

        if (model.LifeSpan < 0)
            errors.Add("Life Span must be a positive number.");

        if (model.PurchaseDate > DateTime.Now)
            errors.Add("Purchase Date cannot be in the future.");
    }

    return errors;
}


```