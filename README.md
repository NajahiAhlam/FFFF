package com.example.cockpit.service;

import com.example.cockpit.dtos.FournisseurDTO;
import com.example.cockpit.dtos.LeadDTO;
import com.example.cockpit.dtos.response.ObjectPagination;
import com.example.cockpit.entities.Fournisseur;
import com.example.cockpit.entities.Lead;
import com.example.cockpit.entities.User;
import com.example.cockpit.repositories.FournisseurRepository;
import org.modelmapper.ModelMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

import java.time.LocalDateTime;
import java.util.List;
import java.util.Objects;
import java.util.Optional;
import java.util.stream.Collectors;

@Service
public class FournisseurServiceImp implements FournisseurService {

    private final FournisseurRepository fournisseurRepository;
    @Autowired
    private ModelMapper modelMapper;

    public FournisseurServiceImp(FournisseurRepository fournisseurRepository){
        this.fournisseurRepository = fournisseurRepository;
    }
    @Override
    public FournisseurDTO save(FournisseurDTO fournisseurDTO) {
        Fournisseur fournisseur = modelMapper.map(fournisseurDTO, Fournisseur.class);
        fournisseur = fournisseurRepository.save(fournisseur);
        return modelMapper.map(fournisseur, FournisseurDTO.class);
    }

    @Override
    public Optional<FournisseurDTO> getOne(Long id) {
        Optional<Fournisseur> fournisseur = fournisseurRepository.findById(id);
        return fournisseur.map(fournisseur1 -> modelMapper.map(fournisseur, FournisseurDTO.class));
    }

    @Override
    public void deleteById(Long id) {
        fournisseurRepository.deleteById(id);

    }

    @Override
    public FournisseurDTO updateOne(Long id, FournisseurDTO fournisseurDTO) {
        Optional<Fournisseur> optionalFournisseur = fournisseurRepository.findById(id);
        if (optionalFournisseur.isPresent()) {
            Fournisseur fournisseur = optionalFournisseur.get();
            fournisseur.setNom(fournisseurDTO.getNom());
            fournisseur.setDescription(fournisseurDTO.getDescription());
            fournisseur.setDateModification(LocalDateTime.now());

            fournisseur = fournisseurRepository.save(fournisseur);
            return modelMapper.map(fournisseur, FournisseurDTO.class);
        } else {
            throw new IllegalArgumentException("Programme with ID " + id + " not found");
        }    }


    @Override
    public ObjectPagination<FournisseurDTO> getAllFournisseur(int page, int size, String sortDirection, String sortValue) {
        Page<Fournisseur> pageOfFournisseur = fournisseurRepository.findAll(PageRequest.of(page, size, Sort.by(Objects.equals(sortDirection, "ASC") ? Sort.Direction.ASC : Sort.Direction.DESC, sortValue)));
        ObjectPagination<FournisseurDTO> pagination = new ObjectPagination<>();
        pagination.setContent(pageOfFournisseur.stream().map(data -> modelMapper.map(data, FournisseurDTO.class)).collect(Collectors.toList()));
        pagination.setLast(pageOfFournisseur.isLast());
        pagination.setFirst(pageOfFournisseur.isFirst());
        pagination.setTotalElements((int) pageOfFournisseur.getTotalElements());
        pagination.setTotalPages(pageOfFournisseur.getTotalPages());
        pagination.setSize(pageOfFournisseur.getSize());
        pagination.setNumber(pageOfFournisseur.getNumber());
        return pagination;
    }

    @Override
    public FournisseurDTO enabledDisabledFournisseur(Long id) {
        Optional<Fournisseur> fournisseurOptional = fournisseurRepository.findById(id);
        if (fournisseurOptional.isPresent()) {
            Fournisseur fournisseur = fournisseurOptional.get();
            fournisseur.setEnabled(!fournisseur.isEnabled());
            fournisseurRepository.save(fournisseur);

            return modelMapper.map(fournisseur, FournisseurDTO.class);
        }
        return null;
    }

    @Override
    public String getFournisseurNameById(Long id) {
        Optional<Fournisseur> fournisseurOptional = fournisseurRepository.findById(id);
        if (fournisseurOptional.isPresent()) {
            Fournisseur fournisseur = fournisseurOptional.get();
            return fournisseur.getNom();
        } else {
            return null;
        }
    }

    @Override
    public List<FournisseurDTO> getAll() {
        List<Fournisseur> fournisseurs = fournisseurRepository.findAll();
        return fournisseurs.stream().map(data-> modelMapper.map(data, FournisseurDTO.class)).collect(Collectors.toList());
    }

    @Override
    public Long count() {
        return fournisseurRepository.count();
    }
}
import { Component } from '@angular/core';
import { NbWindowControlButtonsConfig, NbWindowRef, NbWindowService } from '@nebular/theme';
import { DialogContratComponent } from '../../component/dialog-contrat/dialog-contrat.component';
import { Contrat } from 'src/app/@core/models/contrat.model';
import { AppDataState, DataStateEnum } from 'src/app/@core/state';
import { Observable, catchError, defaultIfEmpty, filter, forkJoin, map, of, startWith, switchMap } from 'rxjs';
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
export class ContratDetailsComponent  {
  requestId!:number
  data$: Observable<AppDataState<Contrat>> | undefined;
  errorMessage!: string;
  contrat!:Contrat
  readonly DataStateEnum = DataStateEnum;
  detailsCardBody:boolean=true;
  detailsFCardBody:boolean=true;
  detailsDatesCardBody:boolean=true;
  fournisseurName$!: Observable<string>;
  userName$!: string;

  constructor(private _route:ActivatedRoute,
    private windowService: NbWindowService,public gestionContratService:GestionContratService, public gestionFournisseurService: GestionFournisseurService, public gestionUserService: GestionUtilisateurService, private _utilService:UtilsService) { }

    ngOnInit(): void {
      this.requestId=this._route.snapshot.params['id']
      this.userName$= this.getUserName(this.contrat.userId)
     this.fournisseurName$= this.getFournisseurName(this.contrat.fournisseurId!)
  
      this.getContratDetails()
    }

    hideShowDetailsCardBody()
    {
      this.detailsCardBody=!this.detailsCardBody
      
    }
    hideShowDetailsFCardBody(){
      this.detailsFCardBody=!this.detailsFCardBody
    }
    hideShowDetailsDatesCardBody(){
      this.detailsDatesCardBody=!this.detailsDatesCardBody
    }
   getContratDetails()
    {
      if(this.requestId)
      {
        this.data$=this.gestionContratService.getContrat(this.requestId)
          .pipe(
            map(response => {
              this.contrat =response;
              return ({dataState: DataStateEnum.LOADED, data: response})
            }),
            startWith({dataState: DataStateEnum.LOADING}),
            catchError(err => of({
              dataState: DataStateEnum.ERROR,
              errorMessage: err.message,
              this: this._utilService.displayError('Une erreur technique est survenue', "Erreur")
            }))
          );
      }
     
    } 
    getFournisseurName(fournisseurId: number){
      return this.gestionFournisseurService.getFournisseurName(fournisseurId).pipe(
        defaultIfEmpty(''),
        map(response => response.toString())
      );
    }
    
    getUserName(userId: number) {
      return this.gestionUserService.getUserName(userId).pipe(
        defaultIfEmpty(''),
        map(response => response.toString())
      );
    }
 openEditForm1() {
    console.log('id',this.requestId)
    console.log('contrat',this.data$)
    //if(!this.contrat){this.getContratDetails();}
    const buttonsConfig: NbWindowControlButtonsConfig = {
      minimize: false,
      maximize: false,
      fullScreen: true,
      close: true,
    };
    
    const nbWindowRef = this.windowService.open(DialogContratComponent, {
      title: `Modifier un contrat`,
      context: { id: this.requestId, contrat: this.contrat  }, 
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
    }
    this.windowService.open(DialogContratComponent, {
      hasBackdrop: true,
      closeOnEsc: false,
      buttons: buttonsConfig,
      closeOnBackdropClick: false,
      title:'Action sur la de modification  du contrat  N°'+this.requestId,
      context:{
        request:this.contrat,
        //withFile:false
      }
    }).onClose.subscribe((data)=>{
      if(data.isEdit)
      {
        //this.upload=true
        this.gestionContratService.updateContrat(this.requestId,this.contrat).subscribe({
          next:(data)=>{
            this._utilService.displaySucess("Succée","Contrat modifié avec succès",)
            location.reload()
            //this.upload=false
          },
          error:(err=>{
            //this.upload=false
          })
        })
      }
    })
  }

  
  private initCloseListener(windowRef: NbWindowRef) {
    windowRef?.onClose.subscribe({
      next: (res) => {
        if (res === 'SUCESS') console.log('succes')
      },

      error: (err) => console.error(`Observer got an error: ${err}`),
    });
  }

 

}
