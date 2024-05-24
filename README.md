It seems like you're experiencing an issue where updating data via Postman results in some fields being saved as `null`. This can be caused by several issues such as mismatched field names, incorrect data types, or issues within the DTO or entity mappings.

Here's a checklist to help debug and resolve this issue:

1. **Check Field Names**:
   Ensure that the field names in your `ContratDTO` match the field names expected by your entity `Contrat`. Any mismatch can result in `null` values being saved.

2. **Validate DTO to Entity Mapping**:
   Verify that your `modelMapper` configuration correctly maps all fields from `ContratDTO` to `Contrat`.

3. **DTO Validation**:
   Ensure that `ContratDTO` is being properly populated before it reaches your service layer. You can add logging to check the values in `contratDTO`.

4. **Check for Null Values in DTO**:
   Add checks to ensure `contratDTO` does not contain `null` values for fields that are required.

5. **Logging**:
   Add logging statements to inspect the values before and after mapping to identify where `null` values might be introduced.

6. **Inspect Request Payload**:
   Verify that the JSON payload sent from Postman contains all the required fields and matches the structure expected by your application.

Here’s an example of how you might add logging to inspect the values and ensure proper mapping:

### Updated Code with Logging

```java
public ContratDTO updateOne(Long id, ContratDTO contratDTO) {
    log.info("Updating Contrat with ID: {}", id);
    log.info("ContratDTO: {}", contratDTO);

    Contrat contrat = contratRepository.findById(id)
            .orElseThrow(() -> new IllegalArgumentException("Contrat with ID " + id + " not found"));

    log.info("Existing Contrat: {}", contrat);

    contrat.setCorrespondant(contratDTO.getCorrespondant());
    contrat.setLeadContrat(contratDTO.getLeadContrat());
    contrat.setContratInitial(contratDTO.getContratInitial());
    contrat.setCommentaire(contratDTO.getCommentaire());
    contrat.setDateSignatureContrat(contratDTO.getDateSignatureContrat());
    contrat.setDateValiditeContrat(contratDTO.getDateValiditeContrat());
    contrat.setDateValiditeKYS(contratDTO.getDateValiditeKYS());
    contrat.setDelaisPaiement(contratDTO.getDelaisPaiement());
    contrat.setDevise(contratDTO.getDevise());
    contrat.setStatus(contratDTO.getStatus());
    contrat.setMontantHt(contratDTO.getMontantHt());
    contrat.setType(contratDTO.getType());
    contrat.setFrequencePaiement(contratDTO.getFrequencePaiement());
    contrat.setFrequencePaiementNom(contratDTO.getFrequencePaiementNom());
    contrat.setFile(contratDTO.getFile());

    if (contratDTO.getFournisseurId() != 0L) {
        Fournisseur fournisseur = fournisseurRepository.findById(contratDTO.getFournisseurId())
                .orElseThrow(() -> new IllegalArgumentException("Fournisseur with ID " + contratDTO.getFournisseurId() + " not found"));
        contrat.setFournisseur(fournisseur);
    }

    if (contratDTO.getUserId() != 0L) {
        User user = userRepository.findById(contratDTO.getUserId())
                .orElseThrow(() -> new IllegalArgumentException("User with ID " + contratDTO.getUserId() + " not found"));
        contrat.setUser(user);
    }

    log.info("Updated Contrat: {}", contrat);

    contratRepository.save(contrat);

    ContratDTO updatedContratDTO = modelMapper.map(contrat, ContratDTO.class);
    log.info("Mapped Updated ContratDTO: {}", updatedContratDTO);

    return updatedContratDTO;
}
```

### Steps to Debug
1. **Run the Update and Check Logs**:
   - Run the update via Postman and check the logs to ensure all fields are being set correctly.
   
2. **Verify JSON Payload**:
   - Ensure that the JSON payload sent from Postman matches the expected structure and includes all necessary fields.

### Example JSON Payload
Ensure your JSON payload in Postman looks something like this:

```json
{
    "correspondant": "John Doe",
    "leadContrat": "Lead Contrat",
    "contratInitial": "Initial Contrat",
    "commentaire": "Some comment",
    "dateSignatureContrat": "2023-05-24",
    "dateValiditeContrat": "2024-05-24",
    "dateValiditeKYS": "2024-06-24",
    "delaisPaiement": 30,
    "devise": "USD",
    "status": "Active",
    "montantHt": 1000.0,
    "type": "Type A",
    "frequencePaiement": "Monthly",
    "frequencePaiementNom": "Monthly Payment",
    "file": "file_path",
    "fournisseurId": 1,
    "userId": 1
}
```

By following these steps and adding logging, you should be able to identify where the `null` values are being introduced and ensure that all fields are properly updated.
