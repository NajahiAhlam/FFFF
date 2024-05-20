 <thead>
              

            <tr class="bg-danger fw-bold text-light border border-primary">
             
                <td *ngIf="displayColumns.includes('Fournisseur')" (click)="sortBy('Fournisseur')" class=" p-3 border border-end-white">
                    <div class="d-flex justify-content-between">
                      <div style="color:white">
                        Fournisseur
                      </div>
                      <div [ngClass]="(sortValue=='fournisseur.nom')? 'text-white':'d-none'">
                              <span *ngIf="sortDirection">
                              <nb-icon icon="arrow-up-line"></nb-icon>
                              </span>
                        <span *ngIf="!sortDirection">
                              <nb-icon icon="arrow-down-line"></nb-icon>
                              </span>
                      </div>
      
                    </div>
                  </td>
                  <td *ngIf="displayColumns.includes('User')" (click)="sortBy('User')" class=" p-3 border border-end-white">
                    <div class="d-flex justify-content-between">
                      <div style="color:white">
                        Commiditaire
                      </div>
                      <div [ngClass]="(sortValue=='user.firstName')? 'text-white':'d-none'">
                              <span *ngIf="sortDirection">
                              <nb-icon icon="arrow-up-line"></nb-icon>
                              </span>
                        <span *ngIf="!sortDirection">
                              <nb-icon icon="arrow-down-line"></nb-icon>
                              </span>
                      </div>
      
                    </div>
                  </td>
              <td *ngIf="displayColumns.includes('Ref Contart')" (click)="sortBy('refContrat')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    reference Contrat
                  </div>
                  <div [ngClass]="(sortValue=='refContrat')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Nature Contrat')" (click)="sortBy('natureContrat')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    nature Contrat
                  </div>
                  <div [ngClass]="(sortValue=='natureContrat')? 'text-white':'d-none'">
                        <span *ngIf="sortDirection">
                        <nb-icon icon="arrow-up-line"></nb-icon>
                        </span>
                    <span *ngIf="!sortDirection">
                        <nb-icon icon="arrow-down-line"></nb-icon>
                        </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Correspondant')" (click)="sortBy('correspondant')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Correspondant
                  </div>
                  <div [ngClass]="(sortValue=='correspondant')? 'text-white':'d-none'">
                        <span *ngIf="sortDirection">
                        <nb-icon icon="arrow-up-line"></nb-icon>
                        </span>
                    <span *ngIf="!sortDirection">
                        <nb-icon icon="arrow-down-line"></nb-icon>
                        </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Date Validite KYS')" (click)="sortBy('dateValiditeKYS')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Date Validite KYS
                  </div>
                  <div [ngClass]="(sortValue=='dateValiditeKYS')? 'text-white':'d-none'">
                        <span *ngIf="sortDirection">
                        <nb-icon icon="arrow-up-line"></nb-icon>
                        </span>
                    <span *ngIf="!sortDirection">
                        <nb-icon icon="arrow-down-line"></nb-icon>
                        </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Contrat Initial')" (click)="sortBy('ContratInitial')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Contrat Initial
                  </div>
                  <div [ngClass]="(sortValue=='ContratInitial')? 'text-white':'d-none'">
                        <span *ngIf="sortDirection">
                        <nb-icon icon="arrow-up-line"></nb-icon>
                        </span>
                    <span *ngIf="!sortDirection">
                        <nb-icon icon="arrow-down-line"></nb-icon>
                        </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Type')"(click)="sortBy('type')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    type
                  </div>
                  <div [ngClass]="(sortValue=='type')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
           
              <td *ngIf="displayColumns.includes('Statut')" (click)="sortBy('status')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    status
                  </div>
                  <div [ngClass]="(sortValue=='status')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Commantaire')" (click)="sortBy('commantaire')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Commantaire
                  </div>
                  <div [ngClass]="(sortValue=='commantaire')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Date Signature Contrat')" (click)="sortBy('dateSignatureContrat')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    date Signature Contrat
                  </div>
                  <div [ngClass]="(sortValue=='dateSignatureContrat')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Date Validite Contrat')" (click)="sortBy('dateValiditeContrat')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Date Validite Contrat
                  </div>
                  <div [ngClass]="(sortValue=='dateValiditeContrat')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Delais Paiement')" (click)="sortBy('delaisPaiement')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Delais Paiement
                  </div>
                  <div [ngClass]="(sortValue=='delaisPaiement')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Lead Contrat')" (click)="sortBy('leadContrat')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Lead Contrat
                  </div>
                  <div [ngClass]="(sortValue=='leadContrat')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
        
              <td *ngIf="displayColumns.includes('Date de création')" (click)="sortBy('dateCreation')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Date création
                  </div>
                  <div [ngClass]="(sortValue=='dateCreation')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Date de modification')" (click)="sortBy('dateModification')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Date modification
                  </div>
                  <div [ngClass]="(sortValue=='modification')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>

              <td *ngIf="displayColumns.includes('Frequence Paiement')" (click)="sortBy('frequencePaiement')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Frequence Paiement
                  </div>
                  <div [ngClass]="(sortValue=='frequencePaiement')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Frequence Paiement Nom')" (click)="sortBy('frequencePaiementNom')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Frequence Paiement Nom
                  </div>
                  <div [ngClass]="(sortValue=='frequencePaiementNom')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Montant Ht')" (click)="sortBy('montantHt')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Montant Ht
                  </div>
                  <div [ngClass]="(sortValue=='montantHt')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('Devise')" (click)="sortBy('devise')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Devise
                  </div>
                  <div [ngClass]="(sortValue=='devise')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <td *ngIf="displayColumns.includes('File')" (click)="sortBy('file')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    File
                  </div>
                  <div [ngClass]="(sortValue=='file')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td>
              <!-- <td (click)="sortBy('dateStatus')" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div style="color:white">
                    Date mise à jour
                  </div>
                  <div [ngClass]="(sortValue=='dateStatus')? 'text-white':'d-none'">
                          <span *ngIf="sortDirection">
                          <nb-icon icon="arrow-up-line"></nb-icon>
                          </span>
                    <span *ngIf="!sortDirection">
                          <nb-icon icon="arrow-down-line"></nb-icon>
                          </span>
                  </div>
  
                </div>
              </td> -->
              <td style="color:white" colspan="2" class=" p-3 border border-end-white">Actions</td>
            </tr>
            </thead> 

            -------------------------------------------------
            displayColumns: Array<string>=['Fournisseur','User','Ref Contrat','Type',"Date de creation",'Date Signature Contrat','Status','Nature Contrat'];
  columns:Array<string>=[
    'Fournisseur',
    'User',
    "Ref Contrat",
    'Type',
    'Date de creation',
    'Status',
    "Commantaire",
    "Correspondant",
    "Date Validite KYS",
    "Lead Contrat",
    "Date de modification",
    'Nature Contrat',
    "Date Signature Contrat",
    'Nature Contrat',
    'Contrat Initial',
    "Date Validite Contrat",
    "Frequence Paiement",
    "Frequence Paiement Nom",
    "Delais Paiement",
    "Montant Ht",
    "Devise","File"]

  showColumnContextMenu: boolean=false;

   ngOnInit(): void {
      this.initSearchForm();
      const storedColumns = localStorage.getItem("columns");
    if (storedColumns) {
      this.displayColumns = JSON.parse(storedColumns);
    } else {
      localStorage.setItem("columns", JSON.stringify(this.displayColumns));
    }
      this.searchCustomerByKeyword();
    }


    toggleContextMenu() {
      this.showColumnContextMenu = !this.showColumnContextMenu;
    }
  
    showColumn(col: string) {
      if(!this.displayColumns.includes(col))
        this.displayColumns.push(col)
      else
        this.displayColumns.splice(this.displayColumns.indexOf(col),1)
      const storedColumns = localStorage.getItem("columns");
        localStorage.setItem("columns", JSON.stringify(this.displayColumns));
    }
