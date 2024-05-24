FournisseurNameDTO getFournisseurNameById(Long id);
@Override
public FournisseurNameDTO getFournisseurNameById(Long id) {
    Optional<Fournisseur> fournisseurOptional = fournisseurRepository.findById(id);
    if (fournisseurOptional.isPresent()) {
        Fournisseur fournisseur = fournisseurOptional.get();
        return new FournisseurNameDTO(fournisseur.getNom());
    } else {
        return null;
    }
}
@GetMapping("/name/{id}")
public ResponseEntity<FournisseurNameDTO> getFournisseurName(@PathVariable Long id) {
    FournisseurNameDTO fournisseurNameDTO = fournisseurService.getFournisseurNameById(id);
    if (fournisseurNameDTO != null) {
        return ResponseEntity.ok(fournisseurNameDTO);
    } else {
        return ResponseEntity.notFound().build();
    }
}
 getFournisseurName(id: number): Observable<string> {
    return this.http.get<{ nom: string }>(`${this.baseUrl}/name/${id}`).pipe(
      map(response => response.nom)
    );
  }


  ----------------------------------------------------------
  import { Component, OnInit } from '@angular/core';
import { NbWindowControlButtonsConfig, NbWindowRef, NbWindowService } from '@nebular/theme';
import { DialogContratComponent } from '../../component/dialog-contrat/dialog-contrat.component';
import { Contrat } from 'src/app/@core/models/contrat.model';
import { AppDataState, DataStateEnum } from 'src/app/@core/state';
import { Observable, of } from 'rxjs';
import { catchError, map, startWith } from 'rxjs/operators';
import { UtilsService } from 'src/app/@core/services/utils.service';
import { GestionContratService } from '../../services/gestion-contrat.service';
import { ActivatedRoute } from '@angular/router';
import { GestionFournisseurService } from 'src/app/pages/gestion-referentiel/services/gestion-fournisseur.service';
import { GestionUtilisateurService } from 'src/app/pages/gestion-utilisateur/service/gestion-utilisateur.service';

@Component({
  selector: 'app-contrat-details',
  templateUrl: './contrat-details.component.html',
  styleUrls: ['./contrat-details.component.scss']
})
export class ContratDetailsComponent implements OnInit {
  requestId!: number;
  data$: Observable<AppDataState<Contrat>> | undefined;
  errorMessage!: string;
  contrat!: Contrat;
  readonly DataStateEnum = DataStateEnum;
  detailsCardBody = true;
  detailsFCardBody = true;
  detailsDatesCardBody = true;
  fournisseurName$!: Observable<string>;
  userName$!: string;

  constructor(
    private _route: ActivatedRoute,
    private windowService: NbWindowService,
    public gestionContratService: GestionContratService,
    public gestionFournisseurService: GestionFournisseurService,
    public gestionUserService: GestionUtilisateurService,
    private _utilService: UtilsService
  ) { }

  ngOnInit(): void {
    this.requestId = this._route.snapshot.params['id'];
    this.getContratDetails();
  }

  hideShowDetailsCardBody() {
    this.detailsCardBody = !this.detailsCardBody;
  }

  hideShowDetailsFCardBody() {
    this.detailsFCardBody = !this.detailsFCardBody;
  }

  hideShowDetailsDatesCardBody() {
    this.detailsDatesCardBody = !this.detailsDatesCardBody;
  }

  getContratDetails() {
    if (this.requestId) {
      this.data$ = this.gestionContratService.getContrat(this.requestId).pipe(
        map(response => {
          this.contrat = response;
          this.fournisseurName$ = this.getFournisseurName(this.contrat.fournisseurId!);
          this.userName$ = this.getUserName(this.contrat.userId);
          return ({ dataState: DataStateEnum.LOADED, data: response });
        }),
        startWith({ dataState: DataStateEnum.LOADING }),
        catchError(err => of({
          dataState: DataStateEnum.ERROR,
          errorMessage: err.message,
          this: this._utilService.displayError('Une erreur technique est survenue', "Erreur")
        }))
      );
    }
  }

  getFournisseurName(fournisseurId: number): Observable<string> {
    return this.gestionFournisseurService.getFournisseurName(fournisseurId).pipe(
      defaultIfEmpty(''),
      map(response => response.toString())
    );
  }

  getUserName(userId: number): string {
    let userName = '';
    this.gestionUserService.getUserName(userId).subscribe(response => {
      userName = response.toString();
    });
    return userName;
  }

  openEditForm1() {
    const buttonsConfig: NbWindowControlButtonsConfig = {
      minimize: false,
      maximize: false,
      fullScreen: true,
      close: true,
    };
    const nbWindowRef = this.windowService.open(DialogContratComponent, {
      title: `Modifier un contrat`,
      context: { id: this.requestId, contrat: this.contrat },
      buttons: buttonsConfig,
    });
    this.initCloseListener(nbWindowRef);
  }

  openEditForm() {
    const buttonsConfig: NbWindowControlButtonsConfig = {
      minimize: false,
      maximize: false,
      fullScreen: false,
      close: true,
    };
    this.windowService.open(DialogContratComponent, {
      hasBackdrop: true,
      closeOnEsc: false,
      buttons: buttonsConfig,
      closeOnBackdropClick: false,
      title: 'Action sur la modification du contrat N°' + this.requestId,
      context: {
        request: this.contrat,
      }
    }).onClose.subscribe((data) => {
      if (data.isEdit) {
        this.gestionContratService.updateContrat(this.requestId, this.contrat).subscribe({
          next: (data) => {
            this._utilService.displaySucess("Succès", "Contrat modifié avec succès");
            location.reload();
          },
          error: (err => {
            console.error(err);
          })
        });
      }
    });
  }

  private initCloseListener(windowRef: NbWindowRef) {
    windowRef?.onClose.subscribe({
      next: (res) => {
        if (res === 'SUCESS') console.log('Success');
      },
      error: (err) => console.error(`Observer got an error: ${err}`),
    });
  }
}
